## I.E. ERNESTO VILLANUEVA MUÑOZ
### CAJARURO - UTCUBAMBA - AMAZONAS
## Robot Tortuga Versión 01
![Image](https://ieevm.github.io/robottortuga1/v1.jpg)


## DISEÑO EN TINKERCAD
![Image](https://ieevm.github.io/robottortuga1/rtv1_2.png) 

### PIEZAS DEL ROBOT
Chasis del Robot [Ver](https://github.com/ieevm/robottortuga1/blob/gh-pages/01Chasis_rtv1.stl)

Soporte de la Rueda [Ver](https://github.com/ieevm/robottortuga1/blob/gh-pages/02SoporteRueda_rtv1.stl)

Rueda [Ver](https://github.com/ieevm/robottortuga1/blob/gh-pages/03Rueda_rtv1.stl)

Soporte de la Matriz de Led [Ver](https://github.com/ieevm/robottortuga1/blob/gh-pages/04SoporteLed_rtv1.stl)

Soporte del Lapicero [Ver](https://github.com/ieevm/robottortuga1/blob/gh-pages/05SoporteLapicero_rtv1.stl)

## ROBOT GRAFICANDO
![Image](https://ieevm.github.io/robottortuga1/rtv1_1.png) 



## CODIGO EN ARDUINO

```markdown
#include "LedControl.h"     // incluye libreria LedControl
typedef int var;     // handles JavaScript var type

#include <Servo.h>

// matriz

#define demora 250  
LedControl lc=LedControl(2,13,3,1); // crea objeto


// setup servo
int servoPin = 8;
int PEN_DOWN = 0; // angulo del servo cuando el lapicero esta abajo
int PEN_UP = 90;   // angulo del servo cuando el lapicero esta arriba
Servo penServo;


float wheel_dia=65; //    # mm (increase = spiral out)
float wheel_base=115; //    # mm (increase = spiral in, ccw) 
int steps_rev=512; //        # 512 for 64x gearbox, 128 for 16x gearbox
int delay_time=6; //         # time between steps in ms

// Stepper sequence org->pink->blue->yel
int L_stepper_pins[] = {12, 10, 9, 11};
int R_stepper_pins[] = {4, 6, 7, 5};


// Gregorio
int pin2 = 2;
int pin3 = 3;
int p1 = HIGH;
int p2 = HIGH;

bool estado1 = false;
bool estado2 = false;

// matriz de led

byte apagado[8] = {   // array con primer cuadro de animacion de flecha
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000
};


byte flecha_arriba_1[8] = {   // array con primer cuadro de animacion de flecha
  B00000000,
  B00011000,
  B00111100,
  B01111110,
  B11011011,
  B00011000,
  B00011000,
  B00011000
};



byte flecha_derecha_1[8] = {   // array con primer cuadro de animacion de flecha
  B00001000,
  B00011000,
  B00110000,
  B01111111,
  B01111111,
  B00110000,
  B00011000,
  B00001000
};



byte flecha_izquierda_1[8] = {   // array con primer cuadro de animacion de flecha
  B00010000,
  B00011000,
  B00001100,
  B11111110,
  B11111110,
  B00001100,
  B00011000,
  B00010000
};



byte flecha_abajo_1[8] = {   // array con primer cuadro de animacion de flecha
  B00000000,
  B00011000,
  B00011000,
  B11011011,
  B01111110,
  B00111100,
  B00011000,
  B00000000
};




void setup() {
    lc.shutdown(0,false);     // enciende la matriz
  lc.setIntensity(0,4);     // establece brillo
  lc.clearDisplay(0);     // blanquea matriz

  
  randomSeed(analogRead(1)); 
  Serial.begin(9600);
  for(int pin=0; pin<4; pin++){
    pinMode(L_stepper_pins[pin], OUTPUT);
    digitalWrite(L_stepper_pins[pin], LOW);
    pinMode(R_stepper_pins[pin], OUTPUT);
    digitalWrite(R_stepper_pins[pin], LOW);
  }
  penServo.attach(servoPin);
  Serial.println("setup");
  
  penup();
  
  delay(1000);
}

void loop(){
    penup();
   delay(3000);
  for (int count2 = 0; count2 < 4; count2++) {
    for (int count = 0; count < 2; count++) {
      moveForward(100);
      turnRight(60);
      moveForward(100);
      turnRight(120);
    }
    turnRight(90);
  }
 
  while(1);
  void pendown();

}




// ----- HELPER FUNCTIONS -----------
int step(float distance){
  int steps = distance * steps_rev / (wheel_dia * 3.1412); //24.61
  /*
  Serial.print(distance);
  Serial.print(" ");
  Serial.print(steps_rev);
  Serial.print(" ");  
  Serial.print(wheel_dia);
  Serial.print(" ");  
  Serial.println(steps);
  delay(1000);*/
  return steps;  
}


void forward(float distance){
     mostrar_1();        // llama funcion mostrar_1
  int steps = step(distance);
  //Serial.println(steps);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(L_stepper_pins[pin], rev_mask[mask][pin]);
        digitalWrite(R_stepper_pins[pin], fwd_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }
    mostrar_apagado();  
}


void backward(float distance){
  mostrar_a_1(); 
  int steps = step(distance);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(L_stepper_pins[pin], fwd_mask[mask][pin]);
        digitalWrite(R_stepper_pins[pin], rev_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }
  
    mostrar_apagado();  
}


void right(float degrees){
    mostrar_d_1(); 
  float rotation = degrees / 360.0;
  float distance = wheel_base * 3.1412 * rotation;
  int steps = step(distance);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(R_stepper_pins[pin], rev_mask[mask][pin]);
        digitalWrite(L_stepper_pins[pin], rev_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }   
    mostrar_apagado();  
}


void left(float degrees){
   mostrar_i_1(); 
  float rotation = degrees / 360.0;
  float distance = wheel_base * 3.1412 * rotation;
  int steps = step(distance);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(R_stepper_pins[pin], fwd_mask[mask][pin]);
        digitalWrite(L_stepper_pins[pin], fwd_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }  
    mostrar_apagado();   
}


void done(){ // unlock stepper to save battery
  for(int mask=0; mask<4; mask++){
    for(int pin=0; pin<4; pin++){
      digitalWrite(R_stepper_pins[pin], LOW);
      digitalWrite(L_stepper_pins[pin], LOW);
    }
    delay(delay_time);
  }
}


void penup(){
  delay(250);
  Serial.println("PEN_UP()");
  penServo.write(PEN_UP);
  delay(250);
}


void pendown(){
  delay(250);  
  Serial.println("PEN_DOWN()");
  penServo.write(PEN_DOWN);
  delay(250);
}


void circle(float radius){
  circle(radius, 360);
}


void circle(float radius, float extent){
  int sides = 1 + int(4 + abs(radius) / 12.0);
  circle(radius, extent, sides);
}


void circle(float radius, float extent, int sides){
  // based on Python's Turtle circle implementation
  float frac = abs(extent) / 360;
  float w = 1.0 * extent / sides;
  float w2 = 0.5 * w;
  float l = 2.0 * radius * sin(w2 * 3.1412 / 180);
  if (radius < 0){
    l = -l;
    w = -w;
    w2 = -w2;
  }
  left(w2);
  Serial.print("circle: ");
  Serial.print("frac=");
  Serial.print(frac);
  Serial.print(" sides=");
  Serial.print(sides);
  Serial.print(" l=");
  Serial.print(l);
  Serial.print(" w=");
  Serial.print(w);
  Serial.print(" w2=");
  Serial.println(w2);

  for(int i=0; i<sides; i++){
    forward(l);
    left(w);
  }
  right(w2);
}


// ----- Elsa to Turtle commands -----------

void moveForward(float distance){
  forward(distance); 
}


void moveBackward(float distance){
  backward(distance); 
}


void turnLeft(float degrees){
  left(degrees);
}


void turnRight(float degrees){
  right(degrees);
}


void jumpForward(float distance){
  penup();
  moveForward(distance);
  pendown();
}


void jumpBackward(float distance){
  penup();
  moveBackward(distance);
  pendown();
}


void mostrar_d_1(){     // funcion mostrar_1
  for (int i = 0; i < 8; i++)     // bucle itera 8 veces por el array
  {
    lc.setRow(0,i,flecha_derecha_1[i]);  // establece fila con valor de array flecha_arriba_1
  }
}


void mostrar_i_1(){     // funcion mostrar_1
  for (int i = 0; i < 8; i++)     // bucle itera 8 veces por el array
  {
    lc.setRow(0,i,flecha_izquierda_1[i]);  // establece fila con valor de array flecha_arriba_1
  }
}

void mostrar_a_1(){     // funcion mostrar_1
  for (int i = 0; i < 8; i++)     // bucle itera 8 veces por el array
  {
    lc.setRow(0,i,flecha_abajo_1[i]);  // establece fila con valor de array flecha_arriba_1
  }
}


void mostrar_1(){     // funcion mostrar_1
  for (int i = 0; i < 8; i++)     // bucle itera 8 veces por el array
  {
    lc.setRow(0,i,flecha_arriba_1[i]);  // establece fila con valor de array flecha_arriba_1
  }
}

void mostrar_apagado(){     // funcion mostrar_1
  for (int i = 0; i < 8; i++)     // bucle itera 8 veces por el array
  {
    lc.setRow(0,i,apagado[i]);  // establece fila con valor de array flecha_arriba_1
  }
}

```


### Librerias

Librerias a utilizar

```markdown
#include "LedControl.h" 
#include <Servo.h>
```

### Matriz Led

Codigo para el control de la matriz led

```markdown
byte apagado[8] = {   // array con primer cuadro de animacion de flecha
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000,
  B00000000
};


byte flecha_arriba_1[8] = {   // array con primer cuadro de animacion de flecha
  B00000000,
  B00011000,
  B00111100,
  B01111110,
  B11011011,
  B00011000,
  B00011000,
  B00011000
};



byte flecha_derecha_1[8] = {   // array con primer cuadro de animacion de flecha
  B00001000,
  B00011000,
  B00110000,
  B01111111,
  B01111111,
  B00110000,
  B00011000,
  B00001000
};



byte flecha_izquierda_1[8] = {   // array con primer cuadro de animacion de flecha
  B00010000,
  B00011000,
  B00001100,
  B11111110,
  B11111110,
  B00001100,
  B00011000,
  B00010000
};



byte flecha_abajo_1[8] = {   // array con primer cuadro de animacion de flecha
  B00000000,
  B00011000,
  B00011000,
  B11011011,
  B01111110,
  B00111100,
  B00011000,
  B00000000
};

```

## Funciones

Codigo de las funciones del robot

```markdown
  int step(float distance){
  int steps = distance * steps_rev / (wheel_dia * 3.1412); //24.61
  return steps;  
}


void forward(float distance){
  int steps = step(distance);
  Serial.println(steps);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(L_stepper_pins[pin], rev_mask[mask][pin]);
        digitalWrite(R_stepper_pins[pin], fwd_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }
}


void backward(float distance){
  int steps = step(distance);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(L_stepper_pins[pin], fwd_mask[mask][pin]);
        digitalWrite(R_stepper_pins[pin], rev_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }
}


void right(float degrees){
  float rotation = degrees / 360.0;
  float distance = wheel_base * 3.1412 * rotation;
  int steps = step(distance);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(R_stepper_pins[pin], rev_mask[mask][pin]);
        digitalWrite(L_stepper_pins[pin], rev_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }   
}


void left(float degrees){
  float rotation = degrees / 360.0;
  float distance = wheel_base * 3.1412 * rotation;
  int steps = step(distance);
  for(int step=0; step<steps; step++){
    for(int mask=0; mask<4; mask++){
      for(int pin=0; pin<4; pin++){
        digitalWrite(R_stepper_pins[pin], fwd_mask[mask][pin]);
        digitalWrite(L_stepper_pins[pin], fwd_mask[mask][pin]);
      }
      delay(delay_time);
    } 
  }   
}


void done(){ // unlock stepper to save battery
  for(int mask=0; mask<4; mask++){
    for(int pin=0; pin<4; pin++){
      digitalWrite(R_stepper_pins[pin], LOW);
      digitalWrite(L_stepper_pins[pin], LOW);
    }
    delay(delay_time);
  }
}


void penup(){
  delay(250);
  Serial.println("PEN_UP()");
  penServo.write(PEN_UP);
  delay(250);
}


void pendown(){
  delay(250);  
  Serial.println("PEN_DOWN()");
  penServo.write(PEN_DOWN);
  delay(250);
}

```


## Video demostrativo

 [Robot Tortuga 01 Video Demostrativo 01](https://youtu.be/et4Xq_pkTNQ). 
 
 [Robot Tortuga 01 Video Demostrativo 02](https://youtu.be/qHy4baHWx6g).
 


### Institución Educativa Ernesto Villanueva Muñoz

 [Página Web](http://ieevm.edu.pe/). 


### Desarrolador

Gregorio Bautista Oblitas
```markdown
- Ingeniero de Sistemas e Informática
- Especialista en tecnología de la informática educativa
- Licenciado en Educación Matemática y Computación
- Magister en Psicologia Educativa
- Maestria Concluida en Ingenieria de Sistemas con mención en Administración de Empresas
- Doctorando en Gestión Pública y Gobernabilidad
```
