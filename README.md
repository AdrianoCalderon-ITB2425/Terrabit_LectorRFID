# Documentació del Lector RFID

**Disseny i Muntatge de la Pistola Lectora**
Identificació de Crotals Electrònics HDX/FDX-B — ISO 11784/11785

---

## Índex

1. [Material Utilitzat](#1-material-utilitzat)
2. [Esquema de Connexions Elèctriques](#2-esquema-de-connexions-elèctriques)
   - 2.1 [Divisor de tensió (5 V → 3,3 V)](#21-divisor-de-tensió-5-v--33-v)
   - 2.2 [Connexions dels pins](#22-connexions-dels-pins)
3. [Muntatge a la Carcassa Pistola 3D](#3-muntatge-a-la-carcassa-pistola-3d)
   - 3.1 [Capa 1 — Base (capa més baixa): Protoboard i circuit](#31-capa-1--base-capa-més-baixa-protoboard-i-circuit)
   - 3.2 [Capa 2 — Mig: ESP32-WROOM-32](#32-capa-2--mig-esp32-wroom-32)
   - 3.3 [Capa 3 — Superior: Pila 9 V i Mòdul WL-134](#33-capa-3--superior-pila-9-v-i-mòdul-wl-134)
   - 3.4 [Antena — Punta de la pistola](#34-antena--punta-de-la-pistola)
4. [Especificacions Tècniques del Sistema](#4-especificacions-tècniques-del-sistema)
5. [Codi Arduino](#5-codi-arduino)
6. [Notes Addicionals](#6-notes-addicionals)

---

## 1. Material Utilitzat

A continuació es detalla tot el material hardware emprat per construir el lector RFID portàtil, amb el model exacte de cada component.

| Component | Model / Especificacions |
|---|---|
| Microcontrolador | ESP32-WROOM-32 |
| Mòdul lector RFID | WL-134 — 134,2 kHz, ISO 11784/11785, FDX-B / HDX, interfície UART TTL, alimentació 5–9 V |
| Protoboard | Mini protoboard sense soldadura, 170 punts (aprox. 35 × 47 mm) |
| Cables de connexió | Cables Dupont M-F i M-M, AWG 28, colors: vermell, negre, groc, blanc, gris, taronja, rosà |
| Pila | Amazon Basics Alkaline 9 V (6LR61) |
| Resistències | Resistències de pel·lícula metàl·lica 1/4 W: 2× 10 kΩ i 1× 20 kΩ (tolerància 1%) |
| Crotals de mostra | Datamars Crotal Electrònic HDX — rang 941000020869379–941000020869383 (5 unitats, CAJA: 20262103) // Crotal Electrònic FDX — rang 941000020869374–941000020869378 (5 unitats, CAJA: 20262102) |
| Carcassa | Pistola lectora dissenyada en 3D |
| Antena | Bobina de coure laminada circular Ø ∼90 mm inclosa amb el mòdul WL-134 |

---

## 2. Esquema de Connexions Elèctriques

### 2.1 Divisor de tensió (5 V → 3,3 V)

El mòdul WL-134 emet el pin TX a 5 V, però l'ESP32-WROOM-32 només admet 3,3 V als seus pins GPIO. Per adaptar el nivell lògic sense fer malbé l'ESP32, s'ha implementat un divisor de tensió resistiu a la protoboard amb les següents resistències:

| R1 | R2 | Vout calculat | Fórmula |
|---|---|---|---|
| 10 kΩ | 20 kΩ | 3,33 V ✔ | Vout = 5 V × 20k/(10k+20k) |

> **Nota:** La resistència de 20 kΩ s'ha construït connectant en sèrie dues resistències de 10 kΩ, de manera que el circuit físic fa servir un total de tres resistències (3× 10 kΩ).

### 2.2 Connexions dels pins

| Mòdul WL-134 | ESP32 / Protoboard | Observacions |
|---|---|---|
| VCC (5–9 V) | Pila 9 V (+) | Alimentació directa des de la pila |
| GND | GND comú (pila i ESP32) | Massa compartida a la protoboard |
| TX (5 V) | Divisor → GPIO RX ESP32 | Divisor de tensió 10 kΩ + 20 kΩ per baixar a 3,3 V |
| Antena | Bobina circular de coure | Surt per l'obertura de la punta de la carcassa |

---

## 3. Muntatge a la Carcassa Pistola 3D

La carcassa ha estat dissenyada i impresa en 3D en format pistola lectora. La seva forma ergonòmica permet subjectar-la amb una mà mentre s'acosta la punta als crotals a identificar. El muntatge interior s'ha organitzat en tres capes superposades per aprofitar l'espai:

### 3.1 Capa 1 — Base (capa més baixa): Protoboard i circuit

La protoboard mini (170 punts) s'aloja a la zona inferior de la carcassa, a l'interior del mànec. Conté:

- 8 cables Dupont connectats als pins corresponents de l'ESP32 i del mòdul WL-134.
- El divisor de tensió format per les tres resistències (2× 10 kΩ en sèrie = 20 kΩ i 1× 10 kΩ), que adapta el pin TX del mòdul (5 V) al nivell acceptable per l'ESP32 (3,3 V).
- El bus de massa comú que comparteixen la pila, el mòdul i l'ESP32.

### 3.2 Capa 2 — Mig: ESP32-WROOM-32

L'ESP32 es col·loca sobre la protoboard, a la capa intermèdia. Rep alimentació des de la protoboard (via la pila de 9 V, regulada internament) i rep les dades de lectura del mòdul WL-134 pel pin RX a través del divisor de tensió.

### 3.3 Capa 3 — Superior: Pila 9 V i Mòdul WL-134

A la capa superior s'allotja la pila de 9 V (Amazon Basics 6LR61) i el mòdul lector WL-134. Totes dues peces queden encaixades a la part superior del mànec i la zona de transició cap al cap de la pistola.

### 3.4 Antena — Punta de la pistola

L'antena circular de coure (bobina laminada, Ø ∼90 mm, inductància 580 µH) surt per un forat dissenyat a la punta de la carcassa i queda sostinguda per unes pestanyes integrades al disseny 3D. Aquestes pestanyes eviten que l'antena es mogui durant l'ús i mantenen la seva posició perpendicular a l'eix de la pistola, maximitzant el rang de lectura.

---

## 4. Especificacions Tècniques del Sistema

| Paràmetre | Valor |
|---|---|
| Freqüència de lectura | 134,2 kHz |
| Protocol | ISO 11784 / ISO 11785 — FDX-B i HDX |
| Interfície de comunicació | UART TTL (9600 bps) |
| Tensió d'alimentació | 9 V (pila Amazon Basics 6LR61) |
| Tensió de treball del mòdul | 5–9 V |
| Tensió GPIO ESP32 | 3,3 V (protegit per divisor resistiu) |
| Rang de lectura (antena circular) | Prop del crotal: fins a ∼10–15 cm aprox. |
| Crotals compatibles | Datamars HDX i qualsevol crotal ISO 11784/11785 |
| Dimensió antena | Ø ∼90 mm (bobina circular de coure laminada) |

---

## 5. Codi Arduino

```cpp
#define RXD2 16
#define TXD2 17

#include "BluetoothSerial.h"
BluetoothSerial SerialBT;

void setup() {
  Serial.begin(115200);
  Serial2.begin(9600, SERIAL_8N2, RXD2, TXD2);
  Serial.println("--- LECTOR DE CROTALES ACTIVADO ---");
  Serial.println("Esperando crotal...");
  SerialBT.begin("ESP32_Escaner");
}

void loop() {
  if (Serial2.available() > 0) {
    String rawData = "";
    delay(150); // Esperar a que llegue todo el paquete de datos

    while (Serial2.available() > 0) {
      char c = Serial2.read();
      if (isAlphaNumeric(c)) {
        rawData += c;
      }
    }

    // Si recibimos una trama válida de al menos 16 caracteres
    if (rawData.length() >= 16) {

      // 1. Extraemos solo la parte útil de la memoria
      String hexData = rawData.substring(0, 16);

      // 2. Le damos la vuelta a la cadena (el lector la envía invertida)
      String reversedHex = "";
      for (int i = 15; i >= 0; i--) {
        reversedHex += hexData[i];
      }

      // 3. Extraemos y convertimos el Código de País (letras 3 a 5)
      String countryHex = reversedHex.substring(3, 6);
      long countryCode = strtol(countryHex.c_str(), NULL, 16);

      // 4. Extraemos y convertimos el Número del Animal (letras 6 a 15)
      String idHex = reversedHex.substring(6, 16);
      // Usamos unsigned long long para números muy grandes de 64 bits
      unsigned long long animalID = strtoull(idHex.c_str(), NULL, 16);

      // 5. Imprimimos el resultado con formato de 15 dígitos
      char crotalCompleto[20];
      // %03d asegura 3 dígitos para el país, %012llu asegura 12 dígitos para el ID
      sprintf(crotalCompleto, "%03d%012llu", (int)countryCode, animalID);

      Serial.println("\n--------------------------------");
      Serial.print("CROTAL LEIDO: ");
      Serial.println(crotalCompleto);
      SerialBT.println(crotalCompleto);
      Serial.println("--------------------------------");
    }
  }
}
```

---

## 6. Notes Addicionals

- El model exacte del mòdul RFID comprat a AliExpress (ref. 32939768419) correspon al WL-134; el producte actual del mateix enllaç pot diferir.
- La pila de 9 V alimenta directament el mòdul WL-134, el qual regula internament la tensió. L'ESP32 s'alimenta des de la protoboard per la mateixa línia.
- El codi Arduino de l'ESP32 ja està documentat per separat.
- Els crotals de mostra pertanyen al lot Datamars HDX, rang 941000020869379 a 941000020869383 (caixa 20262103).
- En cas de voler substituir la pila per una altra font, cal assegurar una alimentació neta (no commutada) per evitar interferències en la lectura RFID.
