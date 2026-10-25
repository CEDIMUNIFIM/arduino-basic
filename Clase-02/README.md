# 🎛️ Clase 2: Control de Señales Analógicas, PWM y Módulos de Salida

Bienvenido a la segunda sesión del **Workshop de Arduino Básico** impartido por el **CEDIM**. En esta clase aprenderás a manejar señales PWM para controlar la intensidad y el color (LED RGB), la lectura de entradas analógicas con potenciómetros (ADC) y la lógica para controlar displays de 7 segmentos.

---

## 🎨 Material de Clase
* 📊 **Diapositivas de la Presentación:** Exclusivas para alumnos registrados. El enlace a Canva está disponible en la descripción fija del grupo de WhatsApp.
* 🛠️ **Simulador recomendado:** [Tinkercad Circuits](https://www.tinkercad.com/)

---

## 🧰 Componentes Utilizados
* 1x Placa Arduino UNO + Cable USB
* 1x Protoboard
* 1x Display de 7 Segmentos (Cátodo Común)
* 1x LED RGB (Cátodo Común)
* 1x Potenciómetro de $10\text{ k}\Omega$
* Resistencias de $220\,\Omega$ / $330\,\Omega$
* Cables Jumper M-M

---

## 🚀 Retos Prácticos de la Clase

### 🔢 Reto 1: Display de 7 Segmentos (Contador Digital)

El objetivo es controlar un display de 7 segmentos usando un arreglo de pines para iterar secuencialmente sobre los segmentos ($a, b, c, d, e, f, g$).

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 2 - Reto 1: Display de 7 Segmentos
*/

// Pines conectados a los segmentos [a, b, c, d, e, f, g]
const int segmentos[7] = {2, 3, 4, 5, 6, 7, 8};

void setup() {
  for (int i = 0; i < 7; i++) {
    pinMode(segmentos[i], OUTPUT);
  }
}

void loop() {
  // Ejemplo: Mostrar el número 8 (todos los segmentos encendidos)
  for (int i = 0; i < 7; i++) {
    digitalWrite(segmentos[i], HIGH);
  }
  delay(1000);

  // Apagar todos los segmentos
  for (int i = 0; i < 7; i++) {
    digitalWrite(segmentos[i], LOW);
  }
  delay(1000);
}
```

---

### 🌈 Reto 2: Control de LED RGB con Modulación PWM (`analogWrite`)

Mediante la técnica de Modulación por Ancho de Pulso (PWM), variamos el ciclo de trabajo en los pines digitales con soporte PWM (`~3`, `~5`, `~6`, `~9`, `~10`, `~11`) con valores de $0$ a $255$ para obtener combinaciones de color.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 2 - Reto 2: Mezcla de Colores con LED RGB
*/

const int PIN_ROJO = 9;
const int PIN_VERDE = 10;
const int PIN_AZUL = 11;

void setup() {
  pinMode(PIN_ROJO, OUTPUT);
  pinMode(PIN_VERDE, OUTPUT);
  pinMode(PIN_AZUL, OUTPUT);
}

void setColor(int rojo, int verde, int azul) {
  analogWrite(PIN_ROJO, rojo);
  analogWrite(PIN_VERDE, verde);
  analogWrite(PIN_AZUL, azul);
}

void loop() {
  setColor(255, 0, 0);   // Rojo puro
  delay(1000);
  setColor(0, 255, 0);   // Verde puro
  delay(1000);
  setColor(0, 0, 255);   // Azul puro
  delay(1000);
  setColor(255, 255, 0); // Amarillo (Rojo + Verde)
  delay(1000);
}
```

---

### 🎚️ Reto 3: Lectura Analógica con Potenciómetro (`analogRead`)

Convertimos el voltaje analógico de entrada en el pin `A0` ($0\text{ V}$ a $5\text{ V}$) en un valor digital de 10 bits ($0$ a $1023$) empleando el Convertidor Analógico Digital (ADC) y enviamos la lectura al Monitor Serie.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 2 - Reto 3: Lectura de Potenciómetro y Puerto Serial
*/

const int PIN_POT = A0;

void setup() {
  Serial.begin(9600); // Inicializa comunicación serie a 9600 baudios
}

void loop() {
  int valorLectura = analogRead(PIN_POT); // Lee valor de 0 a 1023
  
  Serial.print("Lectura del Potenciómetro: ");
  Serial.println(valorLectura);
  
  delay(200); // Pausa breve para estabilizar las lecturas
}
```

---

### 🎲 Reto 4: Secuencia de LEDs con Generación Aleatoria (`random` & `randomSeed`)

Utilizamos la función `random()` para seleccionar de forma pseudoaleatoria los componentes o pines a activar, e inicializamos el generador con `randomSeed(analogRead(A0))` usando una lectura analógica flotante para asegurar una secuencia no repetitiva.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 2 - Reto 4: Secuencia Aleatoria de LEDs
*/

const int leds[3] = {2, 3, 4};

void setup() {
  for (int i = 0; i < 3; i++) {
    pinMode(leds[i], OUTPUT);
  }
  // Utiliza el ruido eléctrico del pin A0 desconectado como semilla
  randomSeed(analogRead(A0));
}

void loop() {
  int indiceAleatorio = random(0, 3); // Deuelve un entero de 0 a 2
  
  digitalWrite(leds[indiceAleatorio], HIGH);
  delay(500);
  digitalWrite(leds[indiceAleatorio], LOW);
  delay(500);
}
```

---

## 🦫 Consejos Capibara
> *"Si la lectura analógica de tu sensor oscila demasiado o da valores inconsistentes, verifica que los terminales de tierra (GND) del potenciómetro y del Arduino estén conectados al mismo bus común."*

---

⬅️ [Volver al índice general del Workshop](../README.md)