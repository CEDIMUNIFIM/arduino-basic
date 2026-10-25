# 🤖 Clase 1: Fundamentos de Arduino y Control Digital

Bienvenido a la primera sesión del **Workshop de Arduino Básico** impartido por el **CEDIM**. En esta clase aprenderás la estructura fundamental de un programa (*sketch*), la configuración del entorno de desarrollo y el manejo de salidas digitales para el control de actuadores simples.

---

## 🎨 Material de Clase
* 📊 **Diapositivas de la Presentación:** Exclusivas para alumnos registrados. El enlace a Canva está disponible en la descripción fija del grupo de WhatsApp.
* 🛠️ **Simulador recomendado:** [Tinkercad Circuits](https://www.tinkercad.com/)

---

## 🧰 Componentes Utilizados
* 1x Placa Arduino UNO (o equivalente) + Cable USB
* 1x Protoboard
* 3x LEDs (Rojo, Amarillo, Verde)
* 3x Resistencias de 220 $\Omega$ o 330 $\Omega$
* Cables Jumper M-M

---

## 🚀 Retos Prácticos de la Clase

### 🔴 Reto 1: Parpadeo de un LED (`Blink`)

El objetivo es encender y apagar el LED integrado en el pin `13` de la placa a intervalos regulares de 1 segundo. Aprendemos el uso de `pinMode()`, `digitalWrite()` y `delay()`.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 1 - Reto 1: Parpadeo de LED (Blink)
*/

const int LED_PIN = 13; // Pin digital conectado al LED

void setup() {
  // Configura el pin digital 13 como salida
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH); // Enciende el LED (5V)
  delay(1000);                  // Espera 1000 ms (1 segundo)
  digitalWrite(LED_PIN, LOW);  // Apaga el LED (0V)
  delay(1000);                  // Espera 1000 ms
}
```

---

### 🚦 Reto 2: Secuencia de Semáforo de Tráfico

Ampliamos el concepto de salidas digitales controlando 3 LEDs de forma secuencial. El programa simula el ciclo de un semáforo vehicular:
1. **Luz Verde:** Encendida durante 3 segundos.
2. **Luz Amarilla:** Encendida durante 1 segundo.
3. **Luz Roja:** Encendida durante 3 segundos.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 1 - Reto 2: Control de Semáforo
*/

// Asignación de pines digitales
const int LED_VERDE = 2;
const int LED_AMARILLO = 3;
const int LED_ROJO = 4;

void setup() {
  // Configuración de pines como salidas
  pinMode(LED_VERDE, OUTPUT);
  pinMode(LED_AMARILLO, OUTPUT);
  pinMode(LED_ROJO, OUTPUT);
}

void loop() {
  // Estado 1: Luz Verde encendida
  digitalWrite(LED_VERDE, HIGH);
  digitalWrite(LED_AMARILLO, LOW);
  digitalWrite(LED_ROJO, LOW);
  delay(3000);

  // Estado 2: Luz Amarilla encendida
  digitalWrite(LED_VERDE, LOW);
  digitalWrite(LED_AMARILLO, HIGH);
  digitalWrite(LED_ROJO, LOW);
  delay(1000);

  // Estado 3: Luz Roja encendida
  digitalWrite(LED_VERDE, LOW);
  digitalWrite(LED_AMARILLO, LOW);
  digitalWrite(LED_ROJO, HIGH);
  delay(3000);
}
```

---

### ⚡ Reto 3: Parpadeo Configurable (Uso de Variables)

Introducimos el concepto de variables en C/C++ para modificar fácilmente los parámetros del sistema. Declaramos una variable `tiempoEspera` que nos permite cambiar la velocidad de destello de múltiples LEDs modificando un solo valor en el código.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 1 - Reto 3: Parpadeo Ajustable con Variables
*/

const int LED1 = 2;
const int LED2 = 3;

// Variable de control de tiempo en milisegundos
int tiempoEspera = 500; 

void setup() {
  pinMode(LED1, OUTPUT);
  pinMode(LED2, OUTPUT);
}

void loop() {
  // Encendido alternado entre LED1 y LED2
  digitalWrite(LED1, HIGH);
  digitalWrite(LED2, LOW);
  delay(tiempoEspera);

  digitalWrite(LED1, LOW);
  digitalWrite(LED2, HIGH);
  delay(tiempoEspera);
}
```

---

## 🦫 Consejos Capibara
> *"Recuerda que C++ es sensible a mayúsculas y minúsculas: `pinMode` no es lo mismo que `Pinmode`. Si la sintaxis cambia de color en el editor, vas por buen camino."*

---

⬅️ [Volver al índice general del Workshop](../README.md)