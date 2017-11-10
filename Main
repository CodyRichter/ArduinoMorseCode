//
// Morse Code Transmitter
//
// @author Cody Richter
// @version 1.0
//

//Pin Configuration
int OUTPUT_PIN_MORSE = 2; //This is the pin # of the pin that will be flashing (LED_BUILTIN for built-in LED on Arduino)
int OUTPUT_PIN_STATIC = 3; //This is the pin # of the pin that will be either on/off
int INPUT_PIN_SENSOR = 52; //This is the pin # of the input pin that contains the input sensor


//User-Controlled Parameters
int BTI = 200; //(Base Time Interval) This is the # of milliseconds which will be used as the base time unit in morse code
char wordToTransmit[] = {'i',' ','l','o','v','e',' ','y','o','u'}; //The word to transmit in morse code, inputted as an array of characters in lower case
int proximityRange = 50; //How close a person must be to the sensor (in inches) for the morse code to activate

//Variable Setup
int letterReference[26][4]; //Array holding all of the values of morse code for each letter in the alphabet. -1 = no code, 0 = short, 1 = long
int arraySize; //Size of Word Array
int sampleWithinRange = 0;
int sampleTotal = 0;

void setup() {
  Serial.begin(9600);
  
  pinMode (OUTPUT_PIN_MORSE, OUTPUT);
  pinMode (OUTPUT_PIN_STATIC, OUTPUT);
  pinMode (INPUT_PIN_SENSOR, INPUT);
  
    //Letter a
  letterReference[0][0] = 0;
  letterReference[0][1] = 1;
  letterReference[0][2] = -1;
  letterReference[0][3] = -1;
    //Letter b
  letterReference[1][0] = 1;
  letterReference[1][1] = 0;
  letterReference[1][2] = 0;
  letterReference[1][3] = 0;
    //Letter c
  letterReference[2][0] = 1;
  letterReference[2][1] = 0;
  letterReference[2][2] = 1;
  letterReference[2][3] = 0;
    //Letter d
  letterReference[3][0] = 1;
  letterReference[3][1] = 0;
  letterReference[3][2] = 0;
  letterReference[3][3] = -1;
    //Letter e
  letterReference[4][0] = 0;
  letterReference[4][1] = -1;
  letterReference[4][2] = -1;
  letterReference[4][3] = -1;
    //Letter f
  letterReference[5][0] = 0;
  letterReference[5][1] = 0;
  letterReference[5][2] = 1;
  letterReference[5][3] = 0;
    //Letter g
  letterReference[6][0] = 1;
  letterReference[6][1] = 1;
  letterReference[6][2] = 0;
  letterReference[6][3] = -1;
    //Letter h
  letterReference[7][0] = 0;
  letterReference[7][1] = 0;
  letterReference[7][2] = 0;
  letterReference[7][3] = 0;
    //Letter i
  letterReference[8][0] = 0;
  letterReference[8][1] = 0;
  letterReference[8][2] = -1;
  letterReference[8][3] = -1;
    //Letter j
  letterReference[9][0] = 0;
  letterReference[9][1] = 1;
  letterReference[9][2] = 1;
  letterReference[9][3] = 1;
    //Letter k
  letterReference[10][0] = 1;
  letterReference[10][1] = 0;
  letterReference[10][2] = 1;
  letterReference[10][3] = -1;
    //Letter l
  letterReference[11][0] = 0;
  letterReference[11][1] = 1;
  letterReference[11][2] = 0;
  letterReference[11][3] = 0;
    //Letter m
  letterReference[12][0] = 1;
  letterReference[12][1] = 1;
  letterReference[12][2] = -1;
  letterReference[12][3] = -1;
    //Letter n
  letterReference[13][0] = 1;
  letterReference[13][1] = 0;
  letterReference[13][2] = -1;
  letterReference[13][3] = -1;
    //Letter o
  letterReference[14][0] = 1;
  letterReference[14][1] = 1;
  letterReference[14][2] = 1;
  letterReference[14][3] = -1;
    //Letter p
  letterReference[15][0] = 0;
  letterReference[15][1] = 1;
  letterReference[15][2] = 1;
  letterReference[15][3] = 0;
    //Letter q
  letterReference[16][0] = 1;
  letterReference[16][1] = 1;
  letterReference[16][2] = 0;
  letterReference[16][3] = 1;
    //Letter r
  letterReference[17][0] = 0;
  letterReference[17][1] = 1;
  letterReference[17][2] = 0;
  letterReference[17][3] = -1;
    //Letter s
  letterReference[18][0] = 0;
  letterReference[18][1] = 0;
  letterReference[18][2] = 0;
  letterReference[18][3] = -1;
    //Letter t
  letterReference[19][0] = 1;
  letterReference[19][1] = -1;
  letterReference[19][2] = -1;
  letterReference[19][3] = -1;
    //Letter u
  letterReference[20][0] = 0;
  letterReference[20][1] = 0;
  letterReference[20][2] = 1;
  letterReference[20][3] = -1;
    //Letter v
  letterReference[21][0] = 0;
  letterReference[21][1] = 0;
  letterReference[21][2] = 0;
  letterReference[21][3] = 1;
    //Letter w
  letterReference[22][0] = 0;
  letterReference[22][1] = 1;
  letterReference[22][2] = 1;
  letterReference[22][3] = -1;
    //Letter x
  letterReference[23][0] = 1;
  letterReference[23][1] = 0;
  letterReference[23][2] = 0;
  letterReference[23][3] = 1;
    //Letter y
  letterReference[24][0] = 1;
  letterReference[24][1] = 0;
  letterReference[24][2] = 1;
  letterReference[24][3] = 1;
    //Letter z
  letterReference[25][0] = 1;
  letterReference[25][1] = 1;
  letterReference[25][2] = 0;
  letterReference[25][3] = 0;

  //Determines Size of Array For Loop Control
  arraySize = sizeof(wordToTransmit)/sizeof(char);
}

void loop() {
  long duration, inches;
  
  //Distance Sensor Scans Area Ahead Of It
  pinMode(INPUT_PIN_SENSOR, OUTPUT);
  digitalWrite(INPUT_PIN_SENSOR, LOW);
  delayMicroseconds(2);
  digitalWrite(INPUT_PIN_SENSOR, HIGH);
  delayMicroseconds(5);
  digitalWrite(INPUT_PIN_SENSOR, LOW);
  //Finish Scanning Area Ahead

  //Calculate Distance To Target
  pinMode(INPUT_PIN_SENSOR, INPUT);
  duration = pulseIn(INPUT_PIN_SENSOR, HIGH);
  inches = microsecondsToInches(duration);
  //Finish Calculating Distance to Target

  if (inches < proximityRange)
  { 
  Serial.print(inches);
  sampleWithinRange++; Serial.println(" Sample: TRUE");}
  else
  { 
  Serial.print(inches); 
  Serial.println(" Sample: FALSE");}
  sampleTotal++;
  if (sampleTotal >= 8)
  {
    if (sampleWithinRange >= (3*sampleTotal)/4)
    {transmitMorseWord(wordToTransmit); Serial.println("[CYCLE COMPLETE] Starting Morse Code");}
    else
    {Serial.println("[CYCLE COMPLETE] NOT Starting Morse Code");}
    sampleTotal = 0;
    sampleWithinRange = 0;
  }
}

void transmitMorseChar(char charToTransmit)
{
  int positionInArray = ((int)charToTransmit)-97;
  
  if (positionInArray == -65) //Will Give Mandatory 7-Tick Delay To Account For Space Between Words
  {
    delay(BTI*7);
    return;
  }
  for (int i = 0; i < 4; i++) {
      if (letterReference[positionInArray][i] == -1)
      {break;}
      //digitalWrite(OUTPUT_PIN_MORSE,HIGH);
      analogWrite(OUTPUT_PIN_MORSE,255);
      if (letterReference[positionInArray][i] == 0) //Delay For Short Period Of Time
      {delay(BTI);}
      else if (letterReference[positionInArray][i] == 1)//Delay For Long Period Of Time
      {delay(BTI*3);}
      //digitalWrite(OUTPUT_PIN_MORSE,LOW);
      analogWrite(OUTPUT_PIN_MORSE,0);

      //Delays For 1 Tick Between Parts Of Letter
      if (i < 3) delay(BTI);
    }
    //Delays For 3 Ticks Between Transmitted Letters
    delay(BTI*3);
  }

void transmitMorseWord(char wordToTransmit[]) {
  Serial.println("Starting Transmit Word");
  for (int i = 0; i < arraySize; i++)
  {
    transmitMorseChar(wordToTransmit[i]);
  }
}

long microsecondsToInches(long microseconds) {
  // According to Parallax's datasheet for the PING))), there are 73.746
  // microseconds per inch (i.e. sound travels at 1130 feet per second).
  // This gives the distance travelled by the ping, outbound and return,
  // so we divide by 2 to get the distance of the obstacle.
  // See: http://www.parallax.com/dl/docs/prod/acc/28015-PING-v1.3.pdf
  return microseconds / 74 / 2;
}
