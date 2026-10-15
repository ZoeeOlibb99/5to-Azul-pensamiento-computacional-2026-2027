# 5to-Azul-pensamiento-computacional-2026-2027
pensamiento computacional

Tarea 3

Problema: Alarma basica de antirrobo de una caja fuerte, gabeta u otros
- El problema es que muy seguido en la contidianidad dejamos una caja fuerte, gabeta, o closet cerrados, resguardando nuestras preciadas pertenencias en ellos, y tenemos que tener una forma de darnos cuenta si esta a salvo o no.
- Cualquier persona se puede beneficiar de un sistema de alarmas como este
- el circuito funciona de forma que la puerta, al abrirse, activará una señal de alerta (una luz LED) para advertir que algun intruso se metio en tus pertenencias.
- Codigo:
- // Definimos los pines que vamos a usar
const int pinBoton = 2;  // El botón simula la puerta
const int pinLed = 13;   // El LED simula la alarma

void setup() {
  // Configuramos el botón como entrada y el LED como salida
  pinMode(pinBoton, INPUT);
  pinMode(pinLed, OUTPUT);
}

void loop() {
  // Leemos el estado del botón
  int estadoPuerta = digitalRead(pinBoton);

  // Si el botón está presionado (puerta abierta)
  if (estadoPuerta == HIGH) {
    digitalWrite(pinLed, HIGH); // Enciende la alarma (LED)
  } else {
    digitalWrite(pinLed, LOW);  // Apaga la alarma (todo seguro)
  }
}

<img width="1085" height="766" alt="image" src="https://github.com/user-attachments/assets/10c9ca9c-9e97-4447-b4b8-f8ea30c12feb" />
