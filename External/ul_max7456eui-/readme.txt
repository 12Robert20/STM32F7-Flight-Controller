Salut! Vreau să continuăm proiectarea plăcii custom de controller de zbor (FC) pentru dronă, bazată pe microcontrolerul STM32F777VIT6 (capsulă LQFP100). 

Iată rezumatul complet al arhitecturii stabilite și stadiul proiectului:

1. COMPONENTE ACTIVE LOGICE (Bifate și complete):
- MCU: STM32F777VIT6 cu cristal extern de 8 MHz și condensatori de 20pF pe pinii OSC_IN/OSC_OUT (PH0/PH1). Are circuitele de boot, reset și condensatorii de decuplare de 100nF pe pinii VDD integrați.
- IMU (Giroscop): LSM6DSV320XTR, conectat pe magistrala dedicată de mare viteză SPI1.
- OSD: MAX7456EUI+ cu rezonator de 27 MHz și rezistențe de 75 Ω pe liniile VIN/VOUT. Pinii HSYNC, VSYNC și LOS sunt lăsați complet în aer (floating/NC). Este conectat pe SPI2.
- BAROMETRU: BMP280 conectat pe I2C1 (cu rezistențe de pull-up la 3.3V_A și pinul SDO la GND pentru adresa 0x76).
- BLACKBOX: Cip de memorie Flash SPI W25Q128JVS (SOIC-8) conectat pe SPI3.
- PERIFERICE EXTERNE: Un socket pentru receptor CRSF (UART) și un socket pentru GPS Sequure M10-122 (UART).
- MOTOARE (DShot600): 4 pini dedicați blocați pe PC6, PC7, PC8, PC9 folosind Timerul 3 (TIM3) în mod electronic modern (Bidirectional DShot pentru telemetrie prin soft, fără fire de TLM/CUR fizice).
- DATE: Mufă USB-C conectată la pinii nativi PA11/PA12 ai STM32 pentru programare în mod DFU.

2. SISTEMUL DE ALIMENTARE (Stabilit):
- Intrare VBAT direct de la ESC-ul extern Holybro Tekko32 F4 Metal 65A.
- Un regulator BUCK (Baterie -> 5V) pentru GPS, Receiver și partea digitală OSD (DVDD).
- Un LDO Digital (5V -> 3.3V_D) pentru pinii VDD ai STM32 și cipul Blackbox.
- Un LDO Analogic (5V -> 3.3V_A) pentru pinul VDDA al STM32, Barometru și partea analogică OSD (AVDD).
- Masă de tip "Star Grounding": Un plan principal de masă digitală (Digital GND) pe toată placa, cu o insulă mică izolată pentru masă analogică (Analog GND) legată printr-un traseu subțire (bridge).

3. CE MAI TREBUIE SĂ FACEM ACUM (Următorii pași):
- Să desenăm și să verificăm schema pentru Blocul de Protecție USB (dioda Schottky pe VBUS și rezistențele de 5.1 kΩ pe pinii CC1/CC2 ai mufei USB-C).
- Să desenăm circuitul ADC (divizorul de tensiune 10kΩ + 1kΩ) de pe linia VBAT către STM32.
- Să facem alocarea pinilor exacți din capsula LQFP100 a STM32 pentru toate magistralele active (SPI1, SPI2, SPI3, I2C1, UART-uri), asigurându-ne că nu există conflicte de timere sau DMA.

Te rog să preiei acest context și să mă guiding în pasul următor!
