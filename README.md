# Sprint 4 - IoT: Monitoramento de Atleta

## Integrantes

| Nome | RM |
| :--- | :--- |
| Mateus Macedo | 563294 |
| Gustavo Cavalcanti | 565601 |
| Matheus Augusto | 565931 |
| Tomás Antonio | 563295 |
| Felipe Riofrio | 563068 |

## Descrição do Projeto

Este projeto consiste em um sistema de monitoramento em tempo real para atletas, utilizando um microcontrolador ESP32. O dispositivo coleta dados de sensores e simuladores para aferir a performance e condição física do atleta, incluindo:

*   **Velocidade:** Simulada através de um potenciômetro.
*   **Batimentos Cardíacos (BPM):** Simulado através de um segundo potenciômetro.
*   **Aceleração (Eixo X):** Medida por um sensor MPU6050.
*   **Temperatura:** Medida pelo sensor de temperatura embutido no MPU6050.

Os dados coletados são enviados via Wi-Fi para um broker MQTT, permitindo que aplicações externas (como dashboards ou sistemas de análise) se inscrevam nos tópicos e recebam as informações em tempo real.

## Arquitetura Proposta

O sistema utiliza uma arquitetura de IoT padrão, onde os sensores e simuladores coletam os dados, o ESP32 os processa e os envia para um broker central através do protocolo MQTT.

### Diagrama de Fluxo

```
[Sensores (MPU6050, Potenciômetros)] --> [ESP32] --> [Roteador Wi-Fi] --> [Internet] --> [Broker MQTT]
```

### Explicação

1.  **Sensores e Potenciômetros**: Capturam os dados de aceleração, temperatura, e simulam os dados de velocidade e batimentos cardíacos.
2.  **ESP32**: Lê os dados dos sensores, estabelece uma conexão com a rede Wi-Fi e com o broker MQTT. Em seguida, publica os dados lidos em tópicos MQTT específicos.
3.  **Roteador Wi-Fi**: Fornece a conexão do ESP32 com a Internet.
4.  **Broker MQTT**: Recebe e distribui as mensagens publicadas pelo ESP32 para quaisquer clientes que estejam inscritos nos tópicos correspondentes.

## Instruções de Uso

### Hardware Necessário

*   Placa de desenvolvimento ESP32
*   Sensor Acelerômetro e Giroscópio MPU6050
*   2 Potenciômetros de 10kΩ
*   Protoboard e Jumpers para as conexões

### Software Necessário

*   Arduino IDE com o suporte para placas ESP32 instalado.
*   Bibliotecas Arduino:
    *   `PubSubClient`
    *   `Adafruit MPU6050`
    *   `Adafruit Unified Sensor`
    *   `WiFi`

### Configuração e Execução

1.  **Montagem do Circuito**: Conecte os componentes ao ESP32.

    *   **MPU6050:**
        *   `VCC` -> `3.3V` do ESP32
        *   `GND` -> `GND` do ESP32
        *   `SCL` -> `GPIO 22` do ESP32
        *   `SDA` -> `GPIO 21` do ESP32
    *   **Potenciômetro de Velocidade:**
        *   Pino central -> `GPIO 33` do ESP32
        *   Pinos laterais -> `3.3V` e `GND`
    *   **Potenciômetro de Batimentos:**
        *   Pino central -> `GPIO 32` do ESP32
        *   Pinos laterais -> `3.3V` e `GND`

2.  **Configuração do Código**:

    *   Abra o arquivo `codigo_fonte_sprint4.cpp` na Arduino IDE.
    *   Altere as constantes no início do arquivo com as suas credenciais de Wi-Fi e informações do broker MQTT, se necessário.

    ````cpp
    const char* default_SSID = "SUA_REDE_WIFI";
    const char* default_PASSWORD = "SUA_SENHA_WIFI";
    const char* default_BROKER_MQTT = "IP_DO_SEU_BROKER";
    const int default_BROKER_PORT = 1883;
    const char* default_TOPICO_SUBSCRIBE = "/ESP/jogador001/cmd";
    ````

3.  **Upload e Monitoramento**:
    *   Conecte o ESP32 ao seu computador.
    *   Na Arduino IDE, selecione a placa "ESP32 Dev Module" (ou similar) e a porta COM correta.
    *   Clique em "Upload" para gravar o código no ESP32.
    *   Abra o "Serial Monitor" com a velocidade de `115200` para acompanhar o status da conexão e os dados que estão sendo publicados no broker MQTT.
    *   Utilize um cliente MQTT (como o MQTT Explorer) para se inscrever nos tópicos e visualizar os dados sendo recebidos em tempo real.
