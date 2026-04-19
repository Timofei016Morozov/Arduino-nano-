# Arduino-nano-
Реле с 2 кнопками 
// Номера пинов для кнопок (3 провода снизу)
const int button1Pin = 2;  // Кнопка 1
const int button2Pin = 3;  // Кнопка 2

// Номера пинов для реле (8 реле сверху)
const int relay1Pin = 4;   // Реле 1
const int relay2Pin = 5;   // Реле 2
const int relay3Pin = 6;   // Реле 3
const int relay4Pin = 7;   // Реле 4
const int relay5Pin = 8;   // Реле 5
const int relay6Pin = 9;   // Реле 6
const int relay7Pin = 10;  // Реле 7
const int relay8Pin = 11;  // Реле 8

// Состояния реле
bool relay1State = false;
bool relay2State = false;
bool relay3State = false;
bool relay4State = false;
bool relay5State = false;
bool relay6State = false;
bool relay7State = false;
bool relay8State = false;

// Переменные для отслеживания времени последнего переключения
unsigned long lastSwitchTime = 0;
const unsigned long switchDelay = 1000; // Задержка 1 секунда

// Переменные для дребезга кнопок
unsigned long lastDebounceTime1 = 0;
unsigned long lastDebounceTime2 = 0;
const unsigned long debounceDelay = 50;

// Состояния кнопок
bool lastButton1State = HIGH;
bool lastButton2State = HIGH;
bool button1Pressed = false;
bool button2Pressed = false;

void setup() {
  // Настройка пинов кнопок (с внутренними подтягивающими резисторами)
  pinMode(button1Pin, INPUT_PULLUP);
  pinMode(button2Pin, INPUT_PULLUP);
  
  // Настройка пинов реле как выходов
  pinMode(relay1Pin, OUTPUT);
  pinMode(relay2Pin, OUTPUT);
  pinMode(relay3Pin, OUTPUT);
  pinMode(relay4Pin, OUTPUT);
  pinMode(relay5Pin, OUTPUT);
  pinMode(relay6Pin, OUTPUT);
  pinMode(relay7Pin, OUTPUT);
  pinMode(relay8Pin, OUTPUT);
  
  // Выключаем все реле при старте
  turnOffAllRelays();
}

void loop() {
  // Чтение состояний кнопок с подавлением дребезга
  checkButton1();
  checkButton2();
  
  // Обработка нажатий
  if (button1Pressed && !button2Pressed && (millis() - lastSwitchTime) >= switchDelay) {
    activateRelay1();
    lastSwitchTime = millis();
    button1Pressed = false;
  }
  
  if (button2Pressed && !button1Pressed && (millis() - lastSwitchTime) >= switchDelay) {
    activateRelay2();
    lastSwitchTime = millis();
    button2Pressed = false;
  }
}

void checkButton1() {
  bool reading = digitalRead(button1Pin);
  
  if (reading != lastButton1State) {
    lastDebounceTime1 = millis();
  }
  
  if ((millis() - lastDebounceTime1) > debounceDelay) {
    if (reading == LOW && !button1Pressed && !button2Pressed) {
      button1Pressed = true;
    }
  }
  
  lastButton1State = reading;
}

void checkButton2() {
  bool reading = digitalRead(button2Pin);
  
  if (reading != lastButton2State) {
    lastDebounceTime2 = millis();
  }
  
  if ((millis() - lastDebounceTime2) > debounceDelay) {
    if (reading == LOW && !button2Pressed && !button1Pressed) {
      button2Pressed = true;
    }
  }
  
  lastButton2State = reading;
}

void activateRelay1() {
  // Выключаем все реле
  turnOffAllRelays();
  
  // Включаем первое реле
  digitalWrite(relay1Pin, HIGH);
  relay1State = true;
}

void activateRelay2() {
  // Выключаем все реле
  turnOffAllRelays();
  
  // Включаем второе реле
  digitalWrite(relay2Pin, HIGH);
  relay2State = true;
}

void turnOffAllRelays() {
  digitalWrite(relay1Pin, LOW);
  digitalWrite(relay2Pin, LOW);
  digitalWrite(relay3Pin, LOW);
  digitalWrite(relay4Pin, LOW);
  digitalWrite(relay5Pin, LOW);
  digitalWrite(relay6Pin, LOW);
  digitalWrite(relay7Pin, LOW);
  digitalWrite(relay8Pin, LOW);
  
  relay1State = false;
  relay2State = false;
  relay3State = false;
  relay4State = false;
  relay5State = false;
  relay6State = false;
  relay7State = false;
  relay8State = false;
}
