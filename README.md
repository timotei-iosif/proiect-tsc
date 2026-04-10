# InkTime

In acest proiect a trebuit implementata o schema electrica data de catre echipa
de TSC in Fusion

## Diagrama bloc

![Diagrama block](Images/block_diagram.png "Diagrama bloc")

## BOM

## Module folosite

### 1. Mcu

Controlerul principal al placii este un nRF52840, bazat pe un nucleu ARM
Cortex-M4F la 64 MHz.

* Clocking: Sistemul utilizeaza doua ceasuri. Un oscilator extern de 32 MHz
  pentru core, periferice si RF, si oscilator de 32.768 kHz este utilizat
  pentru RTC
* Interfata RF: 2450AT18B100E - banda de 2.4 GHz, integrata cu o retea de
  adaptare a impedantei formata din componente pasive
* Interfete de date:
    * **I2C:** Magistrala pentru senzor IMU, fuel gauge, charger, driver haptic
    * **SPI:** Pentru display-ul E-Paper.
    * **USB:** Liniile D+ ai D- sunt rutate direct la pinii nativi ai nRF52840

### 2. Module

* **Incarcare:** Implementat cu un BQ25180. Preia tensiunea de 5V de pe
  magistrala VBUS (USB) si gestioneaza ciclul de incarcare al celulei LiPo.
  Configurabil folosind I2C + PMIC\_INT
* **Regulatoare:** Buck-boost - Richtek RT6160. Rolul sau este sa asigure o
  tensiune stabila de 3.3V
* **Monitorizare:** MAX17048 citeste starea de incarcare a bateriei. Ofera
  alerte hardware la praguri predefinite de descarcare.
* **Afisaj E-Paper**
* **Sistem Haptic:** - DRV2605. Este comandat prin I2C si activat via pinul
  HAPTIC\_EN`.
* **Inertial Measurement Unit (IMU):** BMA421. Liniile de intrerupere
  (`IMU\_INT1`, `IMU\_INT2`) sunt folosite pentru algoritmul de wake-on-tilt.
* **3 Butoane fizice**

## 3. Calcule Estimative de Consum (Power Budget)

1.  **Standby**: ~10 - 20 uA
2.  **Radio activ (BLE TX/RX): ~5 - 15 mA**
3.  **Actualizare Display (Refresh E-Paper):** ~5 - 15 mA (timp de 1-3 secunde)
4.  **Feedback Haptic:** 50 - 100+ mA pentru perioade scurte

## Pini de nRF52840

Conform manualului de referinta a nRF52840 pinii pentru protocoale digitale (cu
exceptia USB si SWD) pot fi rutati la orice pin fizic ai microcontrollerului.
Alegerea pinilor pentru aceste protocoale a fost facuta pentru a facilita
rutarea de semnale.

