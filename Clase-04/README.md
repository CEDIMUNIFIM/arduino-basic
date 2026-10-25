# 🤖 Clase 4: Actuadores de Potencia, Motores y Pantalla LCD

Bienvenido a la cuarta y última sesión del **Workshop de Arduino Básico** impartido por el **CEDIM**. En esta clase aprenderás a controlar actuadores de mayor potencia (motores DC, servomotores y relés), generación de tonos de sonido con zumbadores y el manejo de pantallas LCD 16x2.

---

## 🎨 Material de Clase
* 📊 **Diapositivas de la Presentación:** Exclusivas para alumnos registrados. El enlace a Canva está disponible en la descripción fija del grupo de WhatsApp.
* 🛠️ **Simulador recomendado:** [Tinkercad Circuits](https://www.tinkercad.com/)

---

## 🧰 Componentes Utilizados
* 1x Placa Arduino UNO + Cable USB
* 1x Protoboard y Cables Jumper
* 1x Motorreductor DC
* 1x Driver de motores L298N
* 1x Servomotor (SG90 o similar)
* 1x Zumbador / Buzzer (Pasivo o Activo)
* 1x Módulo Relé de 5 V
* 1x Pantalla LCD 16x2 + Potenciómetro de 10 kΩ
* Batería o Fuente Externa de 9 V

---

## 🚀 Retos Prácticos de la Clase

### ⚙️ Reto 1: Control de Motorreductor DC con Driver L298N

El objetivo es controlar la velocidad y el sentido de giro de un motorreductor utilizando el módulo Puente H L298N mediante señales PWM y digitales.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 4 - Reto 1: Control de Motorreductor DC con L298N
*/

const int IN1 = 7;
const int IN2 = 6;
const int ENA = 9; // Pin PWM para control de velocidad

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(ENA, OUTPUT);
}

void loop() {
  // Configurar sentido de giro horario
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);

  // Aumentar velocidad progresivamente (0 a 255)
  for (int i = 0; i < 255; i++) {
    analogWrite(ENA, i);
    delay(5);
  }

  // Disminuir velocidad progresivamente (255 a 0)
  for (int i = 255; i >= 0; i--) {
    analogWrite(ENA, i);
    delay(5);
  }

  // Invertir el sentido de giro (antihorario)
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);

  // Aumentar velocidad
  for (int i = 0; i < 255; i++) {
    analogWrite(ENA, i);
    delay(5);
  }

  // Disminuir velocidad
  for (int i = 255; i >= 0; i--) {
    analogWrite(ENA, i);
    delay(5);
  }
}
```

---

### 🔄 Reto 2: Control de Posición con Servomotor

Se utiliza la librería `<Servo.h>` para realizar un barrido continuo del eje del servomotor desde 0° hasta 180° y de regreso.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 4 - Reto 2: Control de Servomotor (0° a 180°)
*/

#include <Servo.h>

const int pinServo = 6;
Servo miServo;

void setup() {
  miServo.attach(pinServo); // Asignar pin digital al servomotor
}

void loop() {
  // Barrido de 0 a 180 grados
  for (int i = 0; i <= 180; i++) {
    miServo.write(i);
    delay(20);
  }

  // Barrido de 180 a 0 grados
  for (int i = 180; i >= 0; i--) {
    miServo.write(i);
    delay(20);
  }
}
```

---

### 🔊 Reto 3: Generación de Tonos y Melodía con Zumbador

Aprendemos a utilizar la función `tone()` para generar frecuencias sonoras específicas (Do, Re, Mi) en un buzzer.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 4 - Reto 3: Secuencia Melódica con Zumbador
*/

const int buzzer = 7;

// Frecuencias en Hz de las notas musicales
#define Nota_DO 262
#define Nota_RE 294
#define Nota_MI 330

#define negra 500 // Duración de la nota en ms

void setup() {
  pinMode(buzzer, OUTPUT);
}

void loop() {
  // Toca la nota DO
  tone(buzzer, Nota_DO, negra);
  delay(negra);

  // Toca la nota RE
  tone(buzzer, Nota_RE, negra);
  delay(negra);

  // Toca la nota MI
  tone(buzzer, Nota_MI, negra);
  delay(negra);

  delay(1000); // Pausa de 1 segundo antes de repetir
}
```

---

### 🔌 Reto 4: Control de Potencia con Relé

Utilizamos una salida digital de Arduino para activar la bobina de un relé electromecánico de 5 V, permitiendo conmutar un circuito independiente.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 4 - Reto 4: Conmutación de Relé
*/

const int pinRele = 7;

void setup() {
  pinMode(pinRele, OUTPUT);
  digitalWrite(pinRele, HIGH); // Mantiene desactivado el relé inicialmente
}

void loop() {
  // Activa el relé (enciende la carga)
  digitalWrite(pinRele, LOW);
  delay(2000);

  // Desactiva el relé (apaga la carga)
  digitalWrite(pinRele, HIGH);
  delay(1000);
}
```

---

### 🖥️ Reto 5: Despliegue de Mensajes en Pantalla LCD 16x2

Utilizamos la librería integrada `<LiquidCrystal.h>` para inicializar la pantalla, ubicar el cursor, imprimir texto e implementar un contador dinámico con desplazamiento usando `millis()` y `scrollDisplayLeft()`.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 4 - Reto 5: Contador con Desplazamiento en LCD 16x2
*/

#include <LiquidCrystal.h>

// Definición de pines: RS, EN, D4, D5, D6, D7
LiquidCrystal lcd(8, 9, 4, 5, 6, 7);

void setup() {
  lcd.begin(16, 2); // Inicializar pantalla de 16 columnas x 2 filas
}

void loop() {
  // Fila 0: Mensaje estático
  lcd.setCursor(0, 0);
  lcd.print("Hola, han pasado");

  // Fila 1: Contador en segundos usando millis()
  lcd.setCursor(0, 1);
  lcd.print(millis() / 1000);
  lcd.print(" seg.");

  // Desplaza los caracteres de la pantalla hacia la izquierda
  lcd.scrollDisplayLeft();
  delay(200);
}
```

---

## 🦫 Consejos Capibara
> *"Nunca alimentes motores de gran consumo o relés de alta potencia directamente desde el pin de 5V de tu Arduino si están trabajando bajo carga pesada; utiliza siempre una fuente de alimentación externa compartiendo la misma tierra (GND)."*

---

⬅️ [Volver al índice general del Workshop](../README.md)