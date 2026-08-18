# CLP Livre ESP32-C6

**Um CLP (Controlador Lógico Programável) open source, modular e acessível, construído em torno do ESP32-C6 Super Mini.**

![Licença](https://img.shields.io/badge/license-MIT-blue.svg)
![Status](https://img.shields.io/badge/status-em%20desenvolvimento-orange)
![Plataforma](https://img.shields.io/badge/plataforma-ESP32--C6-purple)

> 📺 Projeto documentado no canal [@thalestgf](https://youtube.com/@thalestgf) — tutoriais completos de eletrônica, PCB e IoT.

---

## 📖 Sobre o projeto

O **CLP Livre** é um controlador lógico programável open source construído em torno do **ESP32-C6 Super Mini**. A placa reúne, em um único módulo, as interfaces mais comuns em automação industrial e residencial: relés, entradas/saídas analógicas padrão indústria (0–10V / 4–20mA), RS485, CAN, LoRa, medição de corrente AC, sensores de temperatura e um barramento I2C isolado galvanicamente para os módulos analógicos.

O projeto é totalmente aberto: esquemático, PCB (KiCad 9.0), firmware e documentação ficam disponíveis para qualquer pessoa estudar, replicar, modificar e melhorar.

## ✨ Principais recursos

- **MCU:** ESP32-C6 Super Mini (Wi-Fi 6, BLE, Zigbee/Thread, RISC-V)
- **Display OLED** embutido (DM-OLED096-636, I2C)
- **2 saídas a relé** (SRA-05VDC-CL, acionadas via MOSFET IRF3205), com contatos NA/NF/COM
- **3 optoacopladores** (EL817) para entradas digitais isoladas / acionamento de servos
- **Entradas analógicas isoladas** (3 canais, 0–10V ou 4–20mA, selecionável por jumper) via INA3221
- **Saída analógica isolada** (corrente ou tensão, selecionável por jumper) via MCP4725 + LM358
- **ADC de propósito geral** ADS1115 (4 canais) para SCT-013, ZMCT101, teclado e potenciômetro
- **Barramento I2C isolado galvanicamente** (ADuM1250) para os módulos de entrada/saída analógica
- **RS485** via MAX485E, com controle de direção por GPIO
- **CAN bus** via MCP2562
- **LoRa** via módulo SX1276 (com conector de antena coaxial)
- **Medição de corrente AC** via SCT-013 e/ou ZMCT101
- **Sensores:** DS18B20 (1-Wire, com proteção TVS), DHT22/DHT11
- **Infravermelho:** receptor (TSDP341xx) e emissor (LED IR SFH4550)
- **Teclado analógico resistivo** de 5 botões (leitura por divisor de tensão em canal único do ADC)
- **Potenciômetro** local (RV1, 10k) para ajuste de setpoint/UI
- **Buzzer** para alertas sonoros
- **Alimentação flexível:** entrada Vin com regulador buck TPS5430 (5V), regulador dedicado 3.3V isolado para o barramento I2C isolado, entrada de bateria 3V6 e entrada de 5V externo, com filtro EMI em ambos os domínios de alimentação

## 🛠️ Hardware

| Item | Descrição |
|---|---|
| MCU | ESP32-C6 Super Mini |
| Display | OLED I2C — DM-OLED096-636 |
| ADC geral | ADS1115IDGS (I2C, 16 bits, 4 canais) |
| ADC isolado (entradas) | INA3221 (I2C, 3 canais) |
| DAC isolado (saída) | MCP4725 (I2C, 12 bits) + LM358 (condicionamento) |
| Isolador I2C | ADuM1250 |
| RS485 | MAX485E |
| CAN | MCP2562-E-SN |
| LoRa | Módulo SX1276 |
| Relés | 2x SRA-05VDC-CL, acionados por IRF3205 |
| Optoacopladores | 3x EL817 |
| Regulador principal | TPS5430 (buck, saída 5V) |
| Regulador I2C isolado | TPS5430 (buck, saída 3.3V) |
| Sensores suportados | DS18B20, DHT22/DHT11, SCT-013, ZMCT101 |
| Interface local | Teclado resistivo 5 botões, potenciômetro, buzzer, IR Rx/Tx |
| Software de projeto | KiCad 9.0 |
| Fabricação | Testado com [JLCPCB](https://jlcpcb.com/?from=thalestgf) |

## 🗺️ Mapa de endereços I2C

| Dispositivo | Endereço | Barramento | Função |
|---|---|---|---|
| ADS1115 (U5, sheet principal) | `0x49` | Principal | Leitura de SCT-013, ZMCT101, teclado analógico, potenciômetro |
| INA3221 (U7) | `0x40` | I2C isolado | 3 entradas analógicas (0–10V / 4–20mA) |
| MCP4725 (U8) | `0x60` | I2C isolado | 1 saída analógica (corrente ou tensão) |
| OLED (U1) | *ver datasheet DM-OLED096-636* | Principal | Display local |

> O barramento I2C isolado se comunica com o principal através do ADuM1250 (U10), garantindo isolação galvânica entre a lógica do ESP32 e os módulos de entrada/saída analógica — importante para proteger o microcontrolador em ambientes industriais com ruído elétrico.

## 🔌 Conectores e terminais

| Conector | Função |
|---|---|
| J1 / J2 | Headers de GPIO expostos do ESP32-C6 (UART, ADC, boot) |
| J3 | Barramento I2C principal |
| J5 | Antena LoRa (coaxial) |
| J6 | Header LoRa |
| J7 | Entradas analógicas isoladas (screw terminal, 6 vias) — AIN0/AIN1/AIN2 |
| J8 | Proteção das entradas analógicas |
| J9 | I2C isolado (header 8 vias) |
| J10 | I2C isolado (screw terminal 2 vias) |
| J11 / J12 | Entrada de alimentação (Vin) |
| J13 | Bateria 3V6 |
| J14 | 5V externo |
| J16 | Vin do regulador do domínio I2C isolado |
| J17 | Entrada ZMCT101 |
| J18 | Entrada SCT-013 |
| J19 | Terminal dos optoacopladores (screw terminal 4 vias) |
| J25 | Saída dos relés (screw terminal 6 vias — NA/NF/COM de RL1 e RL2) |
| J26 | DS18B20 (screw terminal 3 vias) |
| J27 | CAN bus (3 vias) |
| J28 | RS485 (3 vias, A/B/DIR) |

## ⚙️ Configuração das entradas/saídas analógicas

As entradas (J7) e a saída (JP11) analógicas isoladas são configuráveis por jumper:

**Entradas analógicas (JP8/JP9/JP10 — uma por canal):**
- Posição 1: Entrada de corrente, **4 a 20 mA**
- Posição 2: Entrada de tensão, **0 a 10 V**

**Saída analógica (JP11):**
- Posição 1: Saída de corrente
- Posição 2: Saída de tensão

## 🤝 Contribuindo

Contribuições são muito bem-vindas! Sinta-se à vontade para:
- Abrir *issues* reportando bugs ou sugerindo melhorias
- Enviar *pull requests* com correções, novos exemplos ou traduções
- Compartilhar seu projeto usando o CLP Livre

## 📜 Licença

Este projeto é distribuído sob a licença **MIT** (hardware e firmware). Veja o arquivo [`LICENSE`](./LICENSE) para mais detalhes.

## 📢 Contato e comunidade

- 📺 YouTube: [@thalestgf](https://youtube.com/@thalestgf)
- 📸 Instagram: [@thales.tgf](https://instagram.com/thalestgf)
- 🎓 Cursos: [thalesferreira.com/cursos](https://thalesferreira.com/cursos)

---

*Se este projeto foi útil pra você, considere deixar uma ⭐ no repositório — isso ajuda outras pessoas a encontrarem o CLP Livre.*
