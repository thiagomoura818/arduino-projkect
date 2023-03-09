
#include <SoftwareSerial.h>
#include <Wire.h> //INCLUSÃO DA BIBLIOTECA
#include "RTClib.h" //INCLUSÃO DA BIBLIOTECA
#include <EEPROM.h>

RTC_DS3231 rtc; //OBJETO DO TIPO RTC_DS3231

int h1 = 0, m1 = 0, d1 = 0;
int status = 0;
int id = 0;
int x = 0;
char conf= 'd';

void setup() {
  pinMode(4, OUTPUT);
  Serial.begin(9600);

  if(EEPROM[0] != 9){
    EEPROM[0] = 9;
    int i = 1;
    while(i <= 3){
      EEPROM[i] = 0;
      i++;
    }
  }else{
    conf = ' ';
    h1 = EEPROM[1];
    m1 = EEPROM[2];
    d1 = EEPROM[3];
  }
  if(! rtc.begin()) { // SE O RTC NÃO FOR INICIALIZADO, FAZ
    Serial.println("DS3231 não encontrado"); //IMPRIME O TEXTO NO MONITOR SERIAL
    while(1); //SEMPRE ENTRE NO LOOP
  }

  if(rtc.lostPower()){ //SE RTC FOI LIGADO PELA PRIMEIRA VEZ / FICOU SEM ENERGIA / ESGOTOU A BATERIA, FAZ
    Serial.println("DS3231 OK!"); //IMPRIME O TEXTO NO MONITOR SERIAL
    //REMOVA O COMENTÁRIO DE UMA DAS LINHAS ABAIXO PARA INSERIR AS INFORMAÇÕES ATUALIZADAS EM SEU RTC
    //rtc.adjust(DateTime(F(__DATE__), F(__TIME__))); //CAPTURA A DATA E HORA EM QUE O SKETCH É COMPILADO
    //rtc.adjust(DateTime(2018, 9, 29, 15, 00, 45)); //(ANO), (MÊS), (DIA), (HORA), (MINUTOS), (SEGUNDOS)
  }
  delay(100); //INTERVALO DE 100 MILISSEGUNDOS
}

// void loop
void loop() {
  DateTime now = rtc.now();
  if(conf=='d'){
  status = 0;
  x = 0;
  h1=0;
  m1=0;
  d1=0;
  digitalWrite(4,LOW);

  Serial.print("\nEntre com o primeiro horario(HORA): ");
  while (h1 == 0) {
    if (Serial.available() > 0) {
      h1 = Serial.parseInt();
      EEPROM[1] = h1;
    }
  }

  Serial.print(h1);
  delay(1000);

  Serial.print("\nEntre com o primeiro horario(MINUTOS): ");
  while (m1 == 0) {
    if (Serial.available() > 0) {
      m1 = Serial.parseInt();
      EEPROM[2] = m1;
    }
  }

  Serial.print(m1);
  delay(1000);

  Serial.print("\nEntre com a duração do irrigador ligado (MINUTOS): ");
  while (d1 == 0) {
    if (Serial.available() > 0) {
      d1 = Serial.parseInt();
      EEPROM[3] = d1;
    }
  }

  Serial.print(d1);
  delay(1000);
  conf=' ';
  }
  
  conf=Serial.read();
  if (Serial.available() > 0) {
    if (conf == 'a') {
      status = 1;
	Serial.print("\nMotor ligado!");
    } else if (conf == 'b') {
      status = 0;
      Serial.print("\nMotor desligado!");
    }
  }


  if (now.hour() == h1 && now.minute() == m1) {
    status = 1;
    id = 1;
    Serial.print("\nMotor ligado!");
  }

  if (status == 1 && id == 1) {
    x++;
    Serial.println(x);
    if (x == (d1 * 60)) {
      status = 0;
      x = 0;
      Serial.print("\nMotor desligado!");
    }
  }

  Serial.print(" / Horas: "); //IMPRIME O TEXTO NA SERIAL
  Serial.print(now.hour(), DEC); //IMPRIME NO MONITOR SERIAL A HORA
  Serial.print(':'); //IMPRIME O CARACTERE NO MONITOR SERIAL
  Serial.print(now.minute(), DEC); //IMPRIME NO MONITOR SERIAL OS MINUTOS
  Serial.print(':'); //IMPRIME O CARACTERE NO MONITOR SERIAL
  Serial.print(now.second(), DEC); //IMPRIME NO MONITOR SERIAL OS SEGUNDOS
  Serial.println(); //QUEBRA DE LINHA NA SERIAL
  digitalWrite(4, status);
  delay(1000);
}

