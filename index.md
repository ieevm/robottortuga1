## Robot Tortuga Versión 01  - I.E. ERNESTO VILLANUEVA MUÑOZ - CAJARURO - UTCUBAMBA - AMAZONAS
![Image](https://ieevm.github.io/robottortuga1.github.io/v1.jpg)


## DISEÑO EN TINKERCAD
![Image](https://ieevm.github.io/robottortuga1.github.io/rtv1_2.png) 
## GRAFICANDO
![Image](https://ieevm.github.io/robottortuga1.github.io/rtv1_1.png) 

## PIEZAS DEL ROBOT
Chasis del Robot [Ver](https://github.com/ieevm/robottortuga1.github.io/blob/gh-pages/01Chasis_rtv1.stl)

Soporte de la Rueda [Ver](https://github.com/ieevm/robottortuga1.github.io/blob/gh-pages/02SoporteRueda_rtv1.stl)

Rueda [Ver](https://github.com/ieevm/robottortuga1.github.io/blob/gh-pages/03Rueda_rtv1.stl)

Soporte de la Matriz de Led [Ver](https://github.com/ieevm/robottortuga1.github.io/blob/gh-pages/04SoporteLed_rtv1.stl)

Soporte del Lapicero [Ver](https://github.com/ieevm/robottortuga1.github.io/blob/gh-pages/05SoporteLapicero_rtv1.stl)


## CODIGO EN ARDUINO

```markdown


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

### Funciones

Codigo para el control de la matriz led

```markdown


```


Para mas detalle [Vídeo en Youtube]().

### Institución Educativa Ernesto Villanueva Muñoz

 [Página Web](http://ieevm.edu.pe/). 

### Desarrolador

Gregorio Bautista Oblitas
```markdown
- Ingeniero de Sistemas e Informática
- Especialista en tecnología de la informática educativa
- Licenciado en Educación Matemática y Computación
- Magister en Psicologia Educativa
- Maestria Concluida en Ingenieria de Sistemas con mensión en Administración
- Doctorando en Gestión Pública y Gobernabilidad
```
