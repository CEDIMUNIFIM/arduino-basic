# 🔊 Clase 3: Entradas Digitales, Sensores y Emisión de Sonido

Bienvenido a la tercera sesión del **Workshop de Arduino Básico** impartido por el **CEDIM**. En esta clase aprenderás a detectar eventos del entorno mediante entradas digitales (pulsadores/botones), medir distancias con sensores ultrasónicos y generar alertas sonoras empleando un buzzer.

---

## 🎨 Material de Clase
* 📊 **Diapositivas de la Presentación:** Exclusivas para alumnos registrados. El enlace a Canva está disponible en la descripción fija del grupo de WhatsApp.
* 🛠️ **Simulador recomendado:** [Tinkercad Circuits](https://www.tinkercad.com/)

---

## 🧰 Componentes Utilizados
* 1x Placa Arduino UNO + Cable USB
* 1x Protoboard
* 1x Pulsador / Botón (Push Button)
* 1x Resistencia de $10\text{ k}\Omega$ (Pulldown/Pullup)
* 1x Sensor Ultrasónico HC-SR04
* 1x Buzzer Pasivo o Activo
* 1x LED y Resistencia de $220\,\Omega$
* Cables Jumper M-M

---

## 🚀 Retos Prácticos de la Clase

### 🔘 Reto 1: Entradas Digitales con Pulsador (`digitalRead`)

El objetivo es detectar la presión de un botón para controlar el encendido de un LED. Aprendemos la diferencia entre configuraciones de resistencias Pull-up y Pull-down, así como el uso de `digitalRead()`.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 3 - Reto 1: Lectura de Pulsador
*/

const int PIN_BOTON = 2;
const int PIN_LED = 13;

void setup() {
  pinMode(PIN_BOTON, INPUT); // Configurado con resistencia externa Pull-down
  pinMode(PIN_LED, OUTPUT);
}

void loop() {
  int estadoBoton = digitalRead(PIN_BOTON); // Lee HIGH (5V) o LOW (0V)

  if (estadoBoton == HIGH) {
    digitalWrite(PIN_LED, HIGH); // Encender LED si se presiona
  } else {
    digitalWrite(PIN_LED, LOW);  // Apagar LED si se suelta
  }
}
```

---

### 🔊 Reto 2: Generación de Tonos y Melodías (`tone`)

Utilizamos la función `tone(pin, frecuencia, duracion)` para emitir sonidos a diferentes frecuencias utilizando un buzzer.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 3 - Reto 2: Emisión de Sonido con Buzzer
*/

const int PIN_BUZZER = 8;

void setup() {
  pinMode(PIN_BUZZER, OUTPUT);
}

void loop() {
  // Emite una nota DO (262 Hz) por 500 ms
  tone(PIN_BUZZER, 262, 500);
  delay(600);

  // Emite una nota MI (330 Hz) por 500 ms
  tone(PIN_BUZZER, 330, 500);
  delay(600);

  // Emite una nota SOL (392 Hz) por 500 ms
  tone(PIN_BUZZER, 392, 500);
  delay(1000);
}
```

---

### 📏 Reto 3: Medición de Distancia con Sensor Ultrasónico HC-SR04

Emitimos un pulso de disparo (*Trigger*) de $10\,\mu\text{s}$ y medimos el tiempo de retorno del eco (*Echo*) usando `pulseIn()`. Calculamos la distancia en centímetros dividiendo el tiempo por $58.2$ (basado en la velocidad del sonido $343\text{ m/s}$).

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 3 - Reto 3: Sensor Ultrasónico HC-SR04
*/

const int PIN_TRIG = 9;
const int PIN_ECHO = 10;

void setup() {
  Serial.begin(9600);
  pinMode(PIN_TRIG, OUTPUT);
  pinMode(PIN_ECHO, INPUT);
}

void loop() {
  // Generar pulso de 10us en TRIGGER
  digitalWrite(PIN_TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(PIN_TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(PIN_TRIG, LOW);

  // Medir tiempo de respuesta en ECHO (en microsegundos)
  long duracion = pulseIn(PIN_ECHO, HIGH);

  // Calcular distancia en cm
  int distancia = duracion / 58.2;

  Serial.print("Distancia: ");
  Serial.print(distancia);
  Serial.println(" cm");

  delay(200);
}
```

---

### 🚨 Reto 4: Sensor de Proximidad con Alerta Sonora y Visual

Combinamos el sensor ultrasónico, el buzzer y un LED para construir un sistema de parqueo/proximidad: la frecuencia de los pitidos aumenta a medida que un objeto se acerca.

```cpp
/*
  CEDIM - Workshop Arduino Básico
  Clase 3 - Reto 4: Alarma de Proximidad Integrada
*/

const int PIN_TRIG = 9;
const int PIN_ECHO = 10;
const int PIN_BUZZER = 8;
const int PIN_LED = 13;

void setup() {
  pinMode(PIN_TRIG, OUTPUT);
  pinMode(PIN_ECHO, INPUT);
  pinMode(PIN_BUZZER, OUTPUT);
  pinMode(PIN_LED, OUTPUT);
}

void loop() {
  digitalWrite(PIN_TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(PIN_TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(PIN_TRIG, LOW);

  long duracion = pulseIn(PIN_ECHO, HIGH);
  int distancia = duracion / 58.2;

  // Lógica de alarma si la distancia es menor a 20 cm
  if (distancia > 0 && distancia < 20) {
    digitalWrite(PIN_LED, HIGH);
    tone(PIN_BUZZER, 1000); // Tono de alerta de 1000 Hz
    delay(distancia * 10);   // El intervalo disminuye cuanto más cerca esté
    digitalWrite(PIN_LED, LOW);
    noTone(PIN_BUZZER);
    delay(distancia * 10);
  } else {
    digitalWrite(PIN_LED, LOW);
    noTone(PIN_BUZZER);
    delay(100);
  }
}
```

---

## 🦫 Consejos Capibara
> *"Si tu sensor ultrasónico arroja lecturas de `0` o valores incoherentes muy grandes, verifica que el objeto esté en un ángulo perpendicular al sensor y que no sobrepase el rango efectivo ($2\text{ cm}$ a $400\text{ cm}$)."*

---

⬅️ [Volver al índice general del Workshop](../README.md)