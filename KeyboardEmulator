#include <Keyboard.h>

// Set up the keyboard layout array, 7 X 8 matrix
const int keyMatrixUnshifted[7][8] = 
{
    { 48, 49, 50, 51, 52, 53, 54, 55}, //{"0","1","2","3","4","5","6","7"},
    { 56, 57, 58, 59, 44, 45, 46, 47}, //{"8","9",":",";",",","-",".","/"},
    { 64, 97, 98, 99,100,101,102,103}, //{"@","a","b","c","d","e","f","g"},
    {104,105,106,107,108,109,110,111}, //{"h","i","j","k","l","m","n","o"},
    {112,113,114,115,116,117,118,119}, //{"p","q","r","s","t","u","v","w"},
    {120,121,122,218,217,216,215, 32}, //{"x","y","z","UA","DA","LA","RA","SPACE"},
    {176,212,177,  0,  0,  0,  0,  0}  //{"ENTER","CLEAR","BREAK","NU1","NU2","NU3","NU4","SHIFT"}
};
const int keyMatrixShifted[7][8] = 
{
    { 48, 33, 34, 35, 36, 37, 38, 39}, //{"0","!","QUOTE","#","$","%","&","'"},
    { 40, 41, 42, 43, 60, 61, 62, 63}, //{"(",")","*","+","<","=",">","?"},
    { 64, 65, 66, 67, 68, 69, 70, 71}, //{"@","A","B","C","D","E","F","G"},
    { 72, 73, 74, 75, 76, 77, 78, 79}, //{"H","I","J","K","L","M","N","O"},
    { 80, 81, 82, 83, 84, 85, 86, 87}, //{"P","Q","R","S","T","U","V","W"},
    { 88, 89, 90,218,217,216,215, 32}, //{"X","Y","Z","UA","DA","LA","RA","SPACE"},
    {176,212,177,  0,  0,  0,  0,  0}  //{"ENTER","CLEAR","BREAK","NU1","NU2","NU3","NU4","SHIFT"}
};
// Set up which pins to use (rows are output HIGH, cols are input pullup)
const int rowPin[] = {A0, A1, A2, A3, A4, A5, 13}; //to kb pins 1,2,4,5,6,7,8
const int colPin[] = {12, 11, 10, 9, 8, 7, 6, 5}; //to kb pins 9,10,11,12,13,14,15,16

// Set up and initialise the variables
bool keyMatrixOutput[7][8] = 
{
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false}
};
bool keyMatrixStored[7][8] = 
{
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false},
    {false,false,false,false,false,false,false,false}
};
int buttonState = LOW;  
int col = 0;
int row = 0;

// Set the initial state of the pins
void setup() {
  pinMode(colPin[0], INPUT_PULLUP);
  pinMode(colPin[1], INPUT_PULLUP);
  pinMode(colPin[2], INPUT_PULLUP);
  pinMode(colPin[3], INPUT_PULLUP);
  pinMode(colPin[4], INPUT_PULLUP);
  pinMode(colPin[5], INPUT_PULLUP);
  pinMode(colPin[6], INPUT_PULLUP);
  pinMode(colPin[7], INPUT_PULLUP);
  pinMode(rowPin[0], OUTPUT);
  pinMode(rowPin[1], OUTPUT);
  pinMode(rowPin[2], OUTPUT);
  pinMode(rowPin[3], OUTPUT);
  pinMode(rowPin[4], OUTPUT);
  pinMode(rowPin[5], OUTPUT);
  pinMode(rowPin[6], OUTPUT);
  Serial.begin(9600);
  Keyboard.begin();
}

void loop() {
  // Loop through all the rows setting the row pin LOW
  for (row = 0; row < 7; row = row + 1) {
    digitalWrite(rowPin[row], LOW);
    // Loop through all the cols to see if any pin has dropped LOW, if so then it has been pressed
    for (col = 0; col < 8; col = col + 1) {
      buttonState = digitalRead(colPin[col]);
      // record any pressed buttons
      if(buttonState < HIGH){
        keyMatrixOutput[row][col] = true;
      } else {
        keyMatrixOutput[row][col] = false;
      }
    }
    digitalWrite(rowPin[row], HIGH);
  }
  // check if a key has changed and output only if changes
  for (row = 0; row < 7; row = row + 1) {
    for (col = 0; col < 8; col = col + 1) {
      if (keyMatrixOutput[row][col] && !keyMatrixStored[row][col]) { // Is this key pressed and not in the store?
        keyMatrixStored[row][col]=true; // Put it in the store
        if (keyMatrixStored[6][7]) { // Is the shift key pressed?
          Serial.print(keyMatrixShifted[row][col]);
          Keyboard.press(keyMatrixShifted[row][col]);
        } else {
          Serial.print(keyMatrixUnshifted[row][col]);
          Keyboard.press(keyMatrixUnshifted[row][col]);
        }
      } else {
        keyMatrixStored[row][col]=keyMatrixOutput[row][col]; 
      }
    }
  }
  delay(30);
  Keyboard.releaseAll();
}
