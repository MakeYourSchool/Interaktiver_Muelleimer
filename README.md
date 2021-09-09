# Interaktiver Muelleimer
----
Hier ist der Beispiel-Code für den interaktiven Mülleimer, der im Maker Festival 2021 zusammen mit Creators Collective präsentiert wird und nachgebaut werden kann. 

Beim interaktiven Mülleimer handelt es sich um einen Aufsatz für einen handelsüblichen Mülleimer, der sehr motivierend wirkt, Dinge in und nicht neben den Mülleimer zu schmeissen. Viel Spaß beim nachbauen!

## Aufbau

<img src=https://github.com/MakeYourSchool/Interaktiver_Muelleimer/blob/main/Abbildungen/Interaktiver-M%C3%BClleimer.jpg width=600px>

Bildquelle: *Wissenschaft im Dialog*

## Code

```
#include <Servo.h>
#include "Ultrasonic.h"

Ultrasonic ultrasonic(8);
Servo MotorL;
Servo MotorR;

int gemessener_Abstand = 0;
int Punktestand = 0;
int Spruch_Nummer = 0;
int Spielzeit = 0;
int aktuelle_Zeit = 0;
unsigned long Spielstartzeit = 0;

const int Spieldauer = 10000;                         //gesamte Spieldauer in Millisekunden
const int Eimer_Durchmesser = 24;                     //Spielfeld-Bereich im Mülleimer, in dem ein Einwurf erkannt wird. Zahl in Zentimeter



//******************************  Setup()  *******************************************


void setup() {
  MotorL.attach(5);                                   // Initialisierung des linken Motors
  MotorR.attach(6);                                   // Initialisierung des rechten Motors

  MotorLDrive(0);
  MotorRDrive(1);
}



//******************************   Loop()  *******************************************


void loop() {

  gemessener_Abstand = ultrasonic.MeasureInCentimeters();
  aktuelle_Zeit = millis();

  if (gemessener_Abstand <= Eimer_Durchmesser) {      //misst die Lichtschranke einen Gegenstand innerhalb der Mülleimer-Grenzen, dann passiert folgendes:
    if (Spielstartzeit == 0) {                        //Spielstart
      Spielstartzeit = aktuelle_Zeit;
      Punktestand = 0;
    }

    Punktestand += 1;                                 //Punktestand wird hochgezählt
    Spruch_Nummer = random(1, 5);                     //Es wird ein zufälliger Spruch ausgewählt

    MotorLDrive(Spruch_Nummer);
    MotorRDrive(Punktestand);
    delay(500);
  }

  Spielzeit = aktuelle_Zeit - Spielstartzeit;

  if ((Spielzeit > Spieldauer) && (Spielstartzeit != 0)) {
    for (int i = 0; i <= 10; i++) {                   //Wackeln, wenn Spiel vorbei
      MotorLDrive(0);
      delay(100);
      MotorLDrive(0.5);
      delay(100);
    }
    Spielstartzeit = 0;
    Spruch_Nummer = 0;
    delay(2000);                                      //Pause bevor neues Spiel startet
    MotorLDrive(Spruch_Nummer);

  }
  delay(5);                                           //Eine kleine Verschnaufpause. Kann erhöht werden, falls das System instabil erscheint. Eine Verringerung erhöht die Emfpindlichkeit
}



//****************************   weitere Funktionen  **********************************



void MotorLDrive(float n) {
  byte pos = 175 - (n) * 35;
  MotorL.write(pos);
}

void MotorRDrive(float n) {
  byte pos = int(174 - (n - 1) * 8.5);
  MotorR.write(pos);
}

```

