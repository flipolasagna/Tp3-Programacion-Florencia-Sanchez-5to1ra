# ˗ˏˋTrabajo Practico N°3´ˎ˗
## ⿻Consignas:
##### 1. Utilizar LEDs para realizar el efecto "auto fantástico: utilice 4 pulsadores para implementar las siguientes funciones:

###### (a) Al presionar el pulsador 1 se enciende o se apaga el efecto.

###### (b) Al presionar los pulsadores 2 y 3 simultáneamente Baja la velocidad del efecto.
###### (c) Al presionar los pulsadores 2 y 4 simultáneamente Sube la velocidad del efecto.

## ⿻Se forma por:
###### • Arduino nano.

###### • 8 leds.

###### • 4 pulsadores.

###### •8 resistencias de 100Ω

# ˗ˏˋInforme´ˎ˗
 
 ##  ⿻Codigo 
#### » lo primero a realizar fue declarar y definir las macros.
###### ↳ Defino los pines(macros.h)
```c
#define bot1 ((PINC >> 5) & 0x01) // Defino los botones
#define bot2 ((PINC >> 4) & 0x01) // Defino los botones
#define bot3 ((PINC >> 3) & 0x01) // Defino los botones
#define bot4 ((PINC >> 2) & 0x01) // Defino los botones

#define led1 (PORTB |= (1 << PB1)) // Defino Led1
#define led2 (PORTB |= (1 << PB0)) // Defino Led2
#define led3 (PORTD |= (1 << PD2)) // Defino Led3
#define led4 (PORTD |= (1 << PD3)) // Defino Led4
#define led5 (PORTD |= (1 << PD4)) // Defino Led5
#define led6 (PORTD |= (1 << PD5)) // Defino Led6
#define led7 (PORTD |= (1 << PD6)) // Defino Led7
#define led8 (PORTD |= (1 << PD7)) // Defino Led8
```
###### ↳ inicio
```c
#include <Arduino.h>
#include "macros.h"
#include "util/delay.h" // habilito la carpeta del delay

int flag_boton1; // Declaro una flag para un antirebote 
uint8_t t = 70; // declaro la variable t
```
###### ↳ Luego Declaro los botones como entradas y los leds como salida
```c
  DDRC &= ~(1 << PC5); // aca configuro como entrada
  PORTC |= (1 << PC5); // aca prendo el pull up
  DDRC &= ~(1 << PC4); // aca configuro como entrada
  PORTC |= (1 << PC4); // aca prendo el pull up
  DDRC &= ~(1 << PC3); // aca configuro como entrada
  PORTC |= (1 << PC3); // aca prendo el pull up
  DDRC &= ~(1 << PC2); // aca configuro como entrada
  PORTC |= (1 << PC2); // aca prendo el pull up

  DDRB |= (1 << PB1); // En esta linea configuro como salida
  DDRB |= (1 << PB0); // En esta linea configuro como salida
  DDRD |= (1 << PD2); // En esta linea configuro como salida
  DDRD |= (1 << PD3); // En esta linea configuro como salida
  DDRD |= (1 << PD4); // En esta linea configuro como salida
  DDRD |= (1 << PD5); // En esta linea configuro como salida
  DDRD |= (1 << PD6); // En esta linea configuro como salida
  DDRD |= (1 << PD7); // En esta linea configuro como salida
```
###### ↳ creo el vector y la variable contador
```c
 //                            0           1          2              3            4         5             6           7        8         9           10            11          12          13           14           15               
  int8_t efecto_leds[16] = {0b00000001, 0b00000011, 0b00000110, 0b00001100, 0b00011000, 0b00110000, 0b01100000, 0b11000000, 0b10000000,0b11000000, 0b01100000, 0b00110000, 0b00011000, 0b00001100, 0b00000110, 0b00000001};
    // ↑___ Defino y declaro el vector llamandolo efecto_leds y con 16 posiciones(empezando a contar desde el 0) cada posicion esta escrita con numeros binarios.
  int8_t cont = 0;// con la variable contador realizara el progreso de los leds
```
#### » Lo segundo a realizar fue el codigo principal
###### ↳ Comienzo preguntando por el boton1 para que realice el efecto (punto a)
```c
while (1)
  {
    if (bot1 == 0)// pregunto si el boton1 se apreto
    {
      _delay_ms(100);// Utilizo un antirrebote(esperando a que las minimas vibraciones que ocacionan las chapitas del boton se detengan y ahi realmente evaluar el estado del boton)
      while(bot1==0){} //Se queda en un bucle preguntando si se apreto el boton1 +++
      _delay_ms(100); // vuelvo a utilizar un antirebote

      flag_boton1 = ~flag_boton1; // Utilizo esta linea para cambiar el estado de la flag_boton1
    }
    if (flag_boton1 != 0)// Si la falg_boton1 es distinta de 0 (se utiliza distinta de... ya que lo amerita la ~ que se utilizo para cambiar el estado del flag_boton1)
    {
      if (cont >= 15) //  Condiciono al contador no sobrepasar a la posicion 15
      {
        cont = 0; // si llega a la posicion 15 vuelve a la posicion 0
      }
      cont++;// contador para que vaya incrementando las posiciones 
      //ERROR-->PORTD = efecto_leds[cont++];
      PORTD= (PORTD&0b00000011)|(efecto_leds[cont]&0b11111100);// |--> ENMASCARAMIENTO: El enmascaramiento sirve para utilizar distintos pines de distintos puertos(PORTD)
      PORTB= (PORTB&0b11111100)|(efecto_leds[cont]&0b00000011);// |--> ENMASCARAMIENTO: El enmascaramiento sirve para utilizar distintos pines de distintos puertos(PORTB)
     
      // uint8_t i = 0
      // while( i < t;})
      // {
      //   _delay_ms(1);
      //    i++;
      // }

      for (uint8_t i = 0; i < t; i++)// Utilice un for el cual repite como un bucle hasta la condicion
    //for (decalro i= 0 ; condiciono a i; que quiero que haga)
    //    (   inicio  ); ( condicion  ) ; (       avance     )
      {
        _delay_ms(1);// Llamo al delay 
      }
    }else{ // si la condicion de la flag_boton1 no se cumple 
      PORTD=0;// Ambos puertos(por el enmascaramiento) se apagan 
      PORTB=0;// Ambos puertos(por el enmascaramiento) se apagan 
    }
```
###### ↳ Pregunto a los botones correspondientes para disminuir la velocidad del efecto(punto b)
 ```c
 if ((bot2 == 0 || bot3 == 0)) // Condiciono al boton 2 y 3 para que disminuya la velocidad (t) del efecto(vector)
    {

      if (t >= 240)// condiciono a la variable t para que no sobrepase 240 la cual elegi como limite (limite del uint8_t = 255)
      {
        t = 240;// se queda en el valor hasta que no se veulva a modificar 
      }

      t += 40;// Esta linea hace que la velocidad(t) se incremente  y disminuya la velocidad 
    }
```
###### ↳ Pregunto a los botones correspondientes para aumentar la velocidad del efecto(punto c)
 ```c
 if ((bot2 == 0 || bot4 == 0))// condiciono a la variable t para que no sobrepase 70 la cual elegi como limite
    {

      if (t <= 70) // condiciono a la velocidad (t) a no decrementar mas de 70
      {
        t = 70;// se queda en el valor hasta que no se vuelva a modificar
      }

      t -= 40;// Esta linea hace que la velocidad (t) se decremente y aumente la velocidad
    }
  }
}
```
