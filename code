#include <TaskScheduler.h>
#include <TaskSchedulerDeclarations.h>
#include <TaskSchedulerSleepMethods.h>

#include <Servo.h>
#include <Wire.h>
#include <EEPROM.h>
#include "Adafruit_TCS34725softi2c.h"
#include <Pixy2.h>

// for servo
#define LeftSDApin 22
#define LeftSCLpin 23
#define RightSDApin 24
#define RightSCLpin 25
byte SERVO_PIN_LEFT_CONTROL = 12;
byte SERVO_PIN_RIGHT_CONTROL = 11;
#define LEFT 1
#define RIGHT -1

// LED pins
#define redPin 44
#define yellowPin 45
#define greenPin 47
#define bluePin 46
#define whitePin 48

// for calibration
bool calibrate = false;
#define calibrationPin 37
#define togglePin 20
int address = 0;
unsigned long startTimeOffset = 0;

// to track which side
bool home = true;
int probablySwitching = 0;

// for pixycam
Pixy2 pixy;
int lastError = 0;
#define ccc this->pixy->ccc

// macros for colors
#define BLACK 0
#define WHITE 1
#define YELLOW 2
#define ORANGE 3



// to schedule probably switching event
Scheduler ts;
void probablyNotSwitching() { probablySwitching = 0; }
Task t(1500, 1, &probablyNotSwitching);

double WHITE_R = 0.33;
double WHITE_G = 0.33;
double WHITE_B = 0.33;

double ORANGE_R = 0.67;
double ORANGE_G = 0.17;
double ORANGE_B = 0.16;

double YELLOW_R = 0.4;
double YELLOW_G = 0.4;
double YELLOW_B = 0.2;

int blackAvg[2][1] = { {67}, {97} };

int BASE_VELOCITY = 125;

void react();

class Motor {
  private:
    Servo s;
    int velocity;
    int dir;
    bool white, orange, yellow;
    Adafruit_TCS34725softi2c tcs;

  public:
    uint16_t red, green, blue, clr;

    Motor() {}

    Motor(int initialVelocity, byte servoControl, int SDAPin, int SCLPin, int dir = 1) {
      this->velocity = initialVelocity;
      this->s.attach(servoControl);
      this->tcs = Adafruit_TCS34725softi2c(TCS34725_INTEGRATIONTIME_24MS,
        TCS34725_GAIN_1X, SDAPin, SCLPin);
      this->tcs.begin();
      this->dir = dir;
    }

    int getVelocity() { 
      return (int) (1500 - (this -> velocity))/(this->dir);
    }

    void setVelocity(int velocity) {
      int baseVelocity = dir ? 1500 : 1518;
      this->velocity = baseVelocity - velocity * this->dir;
      this->s.writeMicroseconds(this->velocity);
    }

    void getRawColors() {
      this->tcs.getRawData(&this->red, &this->green, &this->blue, &this->clr);
    }
};

class Robot {

  public:
    Motor l;
    Motor r;
    Pixy2* pixy;
    bool lw = false, lb = false, lo = false, ly = false, rw = false, rb = false, ro = false, ry = false;
    int lc = BLACK;
    int rc = BLACK;
    Robot() {}

    Robot(byte LEFT_CONTROL, int LEFT_SDA_PIN, int LEFT_SCL_PIN, byte RIGHT_CONTROL, int RIGHT_SDA_PIN, int RIGHT_SCL_PIN, Pixy2* pixy) {
      this->l = Motor(BASE_VELOCITY, LEFT_CONTROL, LEFT_SDA_PIN, LEFT_SCL_PIN, LEFT);
      this->r = Motor(BASE_VELOCITY, RIGHT_CONTROL, RIGHT_SDA_PIN, RIGHT_SCL_PIN, RIGHT);
			this->pixy = pixy;
    }

    void setVelocity(int v) {
			this->l.setVelocity(v);
			this->r.setVelocity(v);
		}

    void getVelocity(int* lv, int* rv) {
      *lv = this->l.getVelocity();
      *rv = this->r.getVelocity();
    }

    void setVelocity(double lv, double rv) {
      this->l.setVelocity((int) lv);
      this->r.setVelocity((int) rv);
    }

    void backUp(int d = 1000, int v = -1) {
      if (v == -1) {
        this->setVelocity(-1 * BASE_VELOCITY * 2);
        delay(d);
      } else {
        this->setVelocity(v);
        delay(d);
      }
    }

    void rotateLeft() { this->setVelocity(-100, 100); }

    void rotateRight() { this->setVelocity(100, -100); }

    void rotateForNSeconds(int dir = 0, int N = 3000) {
      switch (dir) {
        case 0:
          if (rand() % 2 == 0) this->rotateRight();
          else this->rotateLeft();
          break;
        case LEFT:
          this->rotateLeft();
          break;
        case RIGHT:
          this->rotateRight();
          break;
      }
      delay(N);
    }

		void backUpAndRotate(int backUpDuration = 1000, int rotationDirection = 0, int rotationDuration = 3000, int backUpVelocity = -1) {
			backUp(backUpDuration, backUpVelocity);
			rotateForNSeconds(rotationDirection, rotationDuration);
		}

    void getRawColors() {
      this->l.getRawColors();
      this->r.getRawColors();
    }

    float Square(float val) {
      return val * val;
    }

    float getEuclideanDistance(float r1, float g1, float b1, float r2, float g2, float b2) {
      return Square(r1 - r2) + Square(g1 - g2) + Square(b1 - b2);
    }

    int getNearestColor(float rawR, float rawG, float rawB, float clr) {
      float r = rawR / (rawR + rawG + rawB);
      float g = rawG / (rawR + rawG + rawB);
      float b = rawB / (rawR + rawG + rawB);
      float whiteDistance = this->getEuclideanDistance(r, g, b, WHITE_R, WHITE_G, WHITE_B);
      float orangeDistance = this->getEuclideanDistance(r, g, b, ORANGE_R, ORANGE_G, ORANGE_B);
      float yellowDistance = this->getEuclideanDistance(r, g, b, YELLOW_R, YELLOW_G, YELLOW_B);
      // Serial.println(whiteDistance);
      // Serial.println(orangeDistance);
      // Serial.println(yellowDistance);

      if (whiteDistance < orangeDistance && whiteDistance < yellowDistance) return WHITE;
      else if (yellowDistance < orangeDistance) return YELLOW;
      else return ORANGE;
    }

    void identifyColor() {
      int nearestColorL = this->getNearestColor(this->l.red, this->l.green, this->l.blue, this->l.clr);
      int nearestColorR = this->getNearestColor(this->r.red, this->r.green, this->r.blue, this->r.clr);
      if (this->l.clr < 2 * blackAvg[0][0]) this->lc = BLACK; 
      else this->lc = nearestColorL;
      if (this->r.clr < 2 * blackAvg[1][0]) this->rc = BLACK; 
      else this->rc = nearestColorR;
    }

		void getColorReadings() {
			this->lw = this->lb = this->lo = this->ly = this->rw = this->rb = this->ro= this->ry = false;
			this->getRawColors();
      this->identifyColor();
      this->lb = this->lc == BLACK;
      this->rb = this->rc == BLACK;

      this->lw = this->lc == WHITE;
      this->rw = this->rc == WHITE;

      this->lo = this->lc == ORANGE;
      this->ro = this->rc == ORANGE;

      this->ly = this->lc == YELLOW;
      this->ry = this->rc == YELLOW;

      // Serial.println("Black");
      // Serial.println(this->lb);
      // Serial.println(this->rb);
      // Serial.println("White");
      // Serial.println(this->lw);
      // Serial.println(this->rw);
      // Serial.println("Orange");
      // Serial.println(this->lo);
      // Serial.println(this->ro);
      // Serial.println("Yellow");
      // Serial.println(this->ly);
      // Serial.println(this->ry);
		}

	    void findObject(){
      double PROPORTION_GAIN = 0.3;
      double DERIVATIVE_GAIN = 0.2;
      int error = this->moveTowardsNearestObject();
      int errorChange = error - lastError;
      int leftSpeed = BASE_VELOCITY + -PROPORTION_GAIN * error + -DERIVATIVE_GAIN * errorChange;
      int rightSpeed = BASE_VELOCITY+ +PROPORTION_GAIN * error + DERIVATIVE_GAIN * errorChange;
      lastError = error;
      this->setVelocity(leftSpeed, rightSpeed);
    }

    int moveTowardsNearestObject() {
     ccc.getBlocks();
     int target_object = 0;
     int area = 0;
     if (ccc.numBlocks) {
       for (int i = 0; i < ccc.numBlocks; i++) {
         // 2 and 3 are orange and yellow
         if (ccc.blocks[i].m_width * ccc.blocks[i].m_height > area && ccc.blocks[i].m_signature != 2 && ccc.blocks[i].m_signature != 3 ) {
           target_object = i;
           area = ccc.blocks[i].m_width * ccc.blocks[i].m_height;
         }
       }
       return moveTowardsTheObject(ccc.blocks[target_object].m_x, ccc.blocks[target_object].m_y);
     }
      return 0;
    }


    void findMiddle(){
      double PROPORTION_GAIN = 0.3;
      double DERIVATIVE_GAIN = 0.2;
      int error = this->moveTowardsMiddle();
      int errorChange = error - lastError;
      int leftSpeed = BASE_VELOCITY + -PROPORTION_GAIN * error + -DERIVATIVE_GAIN * errorChange;
      int rightSpeed = BASE_VELOCITY+ +PROPORTION_GAIN * error + DERIVATIVE_GAIN * errorChange;
      lastError = error;
      this->setVelocity(leftSpeed, rightSpeed);
    }

    int moveTowardsMiddle() {
     ccc.getBlocks();
     int target_object = 0;
     int area = 0;
     if (ccc.numBlocks) {
       for (int i = 0; i < ccc.numBlocks; i++) {
         // 2 and 3 are orange and yellow
         if (ccc.blocks[i].m_width * ccc.blocks[i].m_height > area && ccc.blocks[i].m_signature == 2 || ccc.blocks[i].m_signature == 3 ) {
           target_object = i;
           area = ccc.blocks[i].m_width * ccc.blocks[i].m_height;
         }
       }
       return moveTowardsTheObject(ccc.blocks[target_object].m_x, ccc.blocks[target_object].m_y);
     }
      return 0;
    }

    int moveTowardsTheObject(int detected_x, int detected_y) {
      // 158 is the center of the camera
      return detected_x - 158;
    }
};

Robot r;

void setAllPinsLow() {
    digitalWrite(redPin, LOW);
    digitalWrite(greenPin, LOW);
    digitalWrite(bluePin, LOW);
    digitalWrite(whitePin, LOW);
    digitalWrite(yellowPin, LOW);
}

void calibrateRobot() {
  blackAvg[0][0] = 0;
  blackAvg[1][0] = 0;
  for (int i = 0; i < 10; i++) {
    r.getColorReadings();
    blackAvg[0][0] += r.l.clr;
    blackAvg[1][0] += r.r.clr;
  }
  blackAvg[0][0] /= 10;
  blackAvg[1][0] /= 10;
}

void toggleHomePin() {
  home = !home;
  if (home == false) delay(100);
  digitalWrite(bluePin, home ? LOW : HIGH);
  // might need to use 2 tasks, one for each
  // side LEFT and RIGHT
  ts.addTask(t);
  delay(200);
}

Servo brush;

void setup() {

  Serial.begin(9600);
  pixy.init();
  r = Robot(SERVO_PIN_LEFT_CONTROL, LeftSDApin, LeftSCLpin, SERVO_PIN_RIGHT_CONTROL, RightSDApin, RightSCLpin, &pixy);
  brush.attach(3);
  brush.writeMicroseconds(1515);
  r.setVelocity(0);
  setAllPinsLow();
  ts = Scheduler();
  ts.init();
  ts.addTask(t);
  t.enable();
  calibrateRobot();
	home = false;
	toggleHomePin();

  pinMode(calibrationPin, INPUT_PULLUP);
  digitalWrite(whitePin, HIGH);
}
bool wait = true;
unsigned long currentTime;
void loop() {

  if (wait) {
    for (;;) {
      if (digitalRead(calibrationPin) == LOW) {
        digitalWrite(whitePin, LOW);
        startTimeOffset = millis();
        r.setVelocity(BASE_VELOCITY);
        brush.writeMicroseconds(1650);
        wait = false;
        Serial.println(wait);
        break;
      }
    }
  }

  currentTime = millis() - startTimeOffset;
  
  if (currentTime >= 88000)  {
    r.setVelocity(0);
    brush.writeMicroseconds(1515);
  }
  //go to opponent's side once the game starts.
  //should change 20000 to sth else later.. now it's because the robot is not moving until after 9s.
  else if (currentTime <= 25000 && home)  {
    react_beginning();
    
  }
  //after 70s,search for middle line and if already at the home side, drive slow.
  else if (currentTime >= 65000 && currentTime <88000){
     BASE_VELOCITY = 150;
     r.setVelocity(BASE_VELOCITY);
     if (home == false) {react_after();}
     else react_home();
   }
//behavior at the opponent's side.
  else react();
}

//behavior after the first crossing to the opponent's side. The robot will stay in the opponent's side and go after dices.
void react() {
  r.getColorReadings();
  bool bothWhite = r.rw && r.lw;
  bool bothOrange = r.ro && r.lo;
  bool bothYellow = r.ry && r.ly;
  bool bothBlack = r.rb && r.lb;
  digitalWrite(greenPin, r.lo ? HIGH : LOW);
  digitalWrite(yellowPin, r.ly ? HIGH : LOW);
  digitalWrite(redPin, r.ro ? HIGH : LOW);
  digitalWrite(whitePin, r.ry ? HIGH : LOW);
  // if color sensors see both white then the robot
  // should back up for 0.5 second and then rotate
  // for 0.6 seconds
  if (bothWhite) {r.backUpAndRotate(400, 0, 600);}

  // not crossing the middle line
  else if (bothOrange || bothYellow) {
   r.backUpAndRotate(400, 0, 600);
  }
  else {
    // if right sees black and left sees white
    // back up for 0.1s, rotate right for 0.6s
    if (r.rb && r.lw) {r.backUpAndRotate(250, LEFT, 600);}
    // if right sees black and left sees either
    // orange or yellow  back up for 0.1s, rotate right for 0.6s
    else if (r.rb && (r.lo || r.ly)) {
      r.backUpAndRotate(250, LEFT, 600);
      if (probablySwitching == RIGHT) {
        //toggleHomePin();
      } else probablySwitching = LEFT;
    }
    // if right sees white and left sees black
    // back up for 0.1 s and rotate LEFT for 0.6s
    else if (r.rw && r.lb) {r.backUpAndRotate(250, RIGHT, 600);}
    // if right sees white and left sees either
    // yellow or orange, back up ofr 0.1 s and
    // rotate for 0.6s
    else if (r.rw && (r.lo || r.ly)) r.backUpAndRotate(400, 0, 600);
    // if right sees orange and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ro && r.lb) {
    // if right sees orange and left sees black
    // back up for 0.1 s and rotate LEFT for 0.6s
      r.backUpAndRotate(250, RIGHT, 600);
      if (probablySwitching == LEFT) {
        //toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    else if (r.ro && r.lw) r.backUpAndRotate(400, 0, 600);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ro && r.ly)  r.backUpAndRotate(400, 0, 600);
    // if right sees yellow and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ry && r.lb)  {
       r.backUpAndRotate(250, RIGHT, 600);
      if (probablySwitching == LEFT) {
        //toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    // if left sees white and right sees yellow
    // rotate right for 1.5s
    else if (r.ry && r.lw) r.backUpAndRotate(400, 0, 600);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ry && r.lo) r.backUpAndRotate(400, 0, 600);
    // an else should never be needed here
  }
  r.findObject();
  
}

//behavior at the very beginning, the robot will cross the middle line and go to the opponent's side
void react_beginning() {
  r.getColorReadings();
  bool bothWhite = r.rw && r.lw;
  bool bothOrange = r.ro && r.lo;
  bool bothYellow = r.ry && r.ly;
  bool bothBlack = r.rb && r.lb;
  digitalWrite(greenPin, r.lo ? HIGH : LOW);
  digitalWrite(yellowPin, r.ly ? HIGH : LOW);
  digitalWrite(redPin, r.ro ? HIGH : LOW);
  digitalWrite(whitePin, r.ry ? HIGH : LOW);
  
  if (bothWhite) r.backUpAndRotate(500, 0, 600);

  // at the moment letting it cross orange
  // and yellow whenever it comes across them
  else if (bothOrange || bothYellow) {
    toggleHomePin();
  }
  else {
    // if right sees black and left sees white
    // rotate left for 1.5s
    if (r.rb && r.lw) r.backUpAndRotate(200, LEFT, 300);
    // if right sees black and left sees either
    // orange or yellow check if the robot was already
    // switching and if it was, toggle home pin
    else if (r.rb && (r.lo || r.ly)) {
      if (probablySwitching == RIGHT) {
        toggleHomePin();
      } else probablySwitching = LEFT;
    }
    // if right sees white and left sees black
    // rotate RIGHT for 1.5s
    else if (r.rw && r.lb) r.backUpAndRotate(200, RIGHT, 300);
    // if right sees white and left sees either
    // yellow or orange, back up ofr 1 s and
    // rotate for 1.5s
    else if (r.rw && (r.lo || r.ly)) r.backUpAndRotate(200, LEFT, 300);
    // if right sees orange and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ro && r.lb) {
      if (probablySwitching == LEFT) {
        toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    // if left sees white and right sees orange
    // rotate right for 1.5s
    else if (r.ro && r.lw) r.backUpAndRotate(200, RIGHT, 100);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ro && r.ly) toggleHomePin();
    // if right sees yellow and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ry && r.lb)  {
      if (probablySwitching == LEFT) {
        toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    // if left sees white and right sees yellow
    // rotate right for 1.5s
    else if (r.ry && r.lw) r.backUpAndRotate(200, RIGHT, 600);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ry && r.lo) toggleHomePin();
    // an else should never be needed here
    else if (r.ry && r.ly) toggleHomePin();
    else if (r.ro && r.lo) toggleHomePin();
  }
   r.setVelocity(110);
  
}

// behavior after the time set to go back to home side. The robot will look for the middle line and cross back to home side.
void react_after() {
   r.getColorReadings();
  bool bothWhite = r.rw && r.lw;
  bool bothOrange = r.ro && r.lo;
  bool bothYellow = r.ry && r.ly;
  bool bothBlack = r.rb && r.lb;
  digitalWrite(greenPin, r.lo ? HIGH : LOW);
  digitalWrite(yellowPin, r.ly ? HIGH : LOW);
  digitalWrite(redPin, r.ro ? HIGH : LOW);
  digitalWrite(whitePin, r.ry ? HIGH : LOW);
  
  if (bothWhite) r.backUpAndRotate(500, 0, 800);

  // at the moment letting it cross orange
  // and yellow whenever it comes across them
  else if (bothOrange || bothYellow) {
    toggleHomePin();
  }
  else {
    // if right sees black and left sees white
    // rotate left for 1.5s
    if (r.rb && r.lw) r.backUpAndRotate(500, LEFT, 800);
    // if right sees black and left sees either
    // orange or yellow check if the robot was already
    // switching and if it was, toggle home pin
    else if (r.rb && (r.lo || r.ly)) {
      if (probablySwitching == RIGHT) {
        toggleHomePin();
      } else probablySwitching = LEFT;
    } 
    // if right sees white and left sees black
    // rotate RIGHT for 1.5s
    else if (r.rw && r.lb) r.backUpAndRotate(500, RIGHT, 800);
    // if right sees white and left sees either
    // yellow or orange, back up ofr 1 s and
    // rotate for 1.5s
    else if (r.rw && (r.lo || r.ly)) r.backUpAndRotate(500, LEFT, 800);
    // if right sees orange and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ro && r.lb) {
      if (probablySwitching == LEFT) {
        toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    // if left sees white and right sees orange
    // rotate right for 1.5s
    else if (r.ro && r.lw) r.backUpAndRotate(500, RIGHT, 100);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ro && r.ly) toggleHomePin();
    // if right sees yellow and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ry && r.lb)  {
      if (probablySwitching == LEFT) {
        toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    // if left sees white and right sees yellow
    // rotate right for 1.5s
    else if (r.ry && r.lw) r.backUpAndRotate(500, RIGHT, 800);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ry && r.lo) toggleHomePin();
    // an else should never be needed here
  }
   r.findMiddle();
  
}

//behavior at the very end. The robot has successfully go back to our side. Now it wait for the end of the contest.
void react_home() {
  r.setVelocity(75);
   r.getColorReadings();
  bool bothWhite = r.rw && r.lw;
  bool bothOrange = r.ro && r.lo;
  bool bothYellow = r.ry && r.ly;
  bool bothBlack = r.rb && r.lb;
  digitalWrite(greenPin, r.lo ? HIGH : LOW);
  digitalWrite(yellowPin, r.ly ? HIGH : LOW);
  digitalWrite(redPin, r.ro ? HIGH : LOW);
  digitalWrite(whitePin, r.ry ? HIGH : LOW);
  // if color sensors see both white then the robot
  // should back up for 0.5 second and then rotate
  // for 0.6 seconds
  if (bothWhite) r.backUpAndRotate(500, 0, 600);

  // not crossing the middle line
  else if (bothOrange || bothYellow) {
   r.backUpAndRotate(500, 0, 600);
  }
  else {
    // if right sees black and left sees white
    // back up for 0.1s, rotate right for 0.6s
    if (r.rb && r.lw) r.backUpAndRotate(100, LEFT, 600);
    // if right sees black and left sees either
    // orange or yellow  back up for 0.1s, rotate right for 0.6s
    else if (r.rb && (r.lo || r.ly)) {
      r.backUpAndRotate(100, LEFT, 600);
      if (probablySwitching == RIGHT) {
        toggleHomePin();
      } else probablySwitching = LEFT;
    }
    // if right sees white and left sees black
    // back up for 0.1 s and rotate LEFT for 0.6s
    else if (r.rw && r.lb) r.backUpAndRotate(100, RIGHT, 600);
    // if right sees white and left sees either
    // yellow or orange, back up ofr 0.1 s and
    // rotate for 0.6s
    else if (r.rw && (r.lo || r.ly)) r.backUpAndRotate(500, 0, 600);
    // if right sees orange and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ro && r.lb) {
    // if right sees orange and left sees black
    // back up for 0.1 s and rotate LEFT for 0.6s
      r.backUpAndRotate(100, RIGHT, 600);
      if (probablySwitching == LEFT) {
        toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    else if (r.ro && r.lw) r.backUpAndRotate(500, 0, 600);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ro && r.ly)  r.backUpAndRotate(500, 0, 600);
    // if right sees yellow and left sees black 
    // check if the robot was already switching
    // and if it was, toggle home pin
    else if (r.ry && r.lb)  {
       r.backUpAndRotate(100, RIGHT, 600);
      if (probablySwitching == LEFT) {
        toggleHomePin();
      } else probablySwitching = RIGHT;
    }
    // if left sees white and right sees yellow
    // rotate right for 1.5s
    else if (r.ry && r.lw) r.backUpAndRotate(500, 0, 600);
    // at the moment letting it cross orange
    // and yellow whenever it comes across them
    else if (r.ry && r.lo) r.backUpAndRotate(500, 0, 600);
    // an else should never be needed here
  }
  r.setVelocity(60);
  
}
