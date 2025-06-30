# Configure Marlin 2.1.2.5 para Ender 3 v1 com placa Creality v4.2.7 e BLTouch

🇺🇸 English version: [README.md](./README.md)

## 📑 Índice

- [🎯 Hardware Alvo](#️-hardware-alvo)
- [📦 Firmware Pré-compilado](#-firmware-pré-compilado)
- [🔧 Calibração do Z-Offset e Nivelamento Automático da Mesa (BLTouch)](#-calibração-do-z-offset-e-nivelamento-automático-da-mesa-bltouch)
- [🧰 Enviando Comandos via USB](#-enviando-comandos-via-usb)
- [🧩 Como Compilar](#-como-compilar)
- [📚 Referência](#-referência)

Este repositório contém arquivos de configuração personalizados para o firmware Marlin de impressoras 3D.

### 🖨️ Hardware Alvo

- **Impressora**: Creality Ender 3 v1
- **Placa**: Creality V4.2.7 (STM32F103RE)
- **Sensor**: BLTouch
- **Firmware**: Marlin 2.1.2.5

## 📦 Firmware Pré-compilado

Se você não quiser compilar o firmware por conta própria, pode baixar o binário pré-compilado:

- **[Baixar firmware-Marlin-2.1.2.5-Ender3-v1-Creality-v4.2.7-BLTouch.bin](./firmware-Marlin-2.1.2.5-Ender3-v1-Creality-v4.2.7-BLTouch.bin)**


**Como instalar:**
1. Formate um cartão microSD como FAT32.
2. Copie o arquivo `.bin` para a raiz do cartão.
3. Desligue sua Ender 3.
4. Insira o cartão na impressora.
5. Ligue a impressora novamente.
6. Aguarde o processo de flash — a tela ficará em branco por alguns segundos e depois ligará normalmente.
7. O arquivo será deletado automaticamente após um flash bem-sucedido.

Após o flash do firmware, [calibre o Z-offset e o nivelamento da mesa](#-calibração-do-z-offset-e-nivelamento-automático-da-mesa-bltouch).

## 🔧 Calibração do Z-Offset e Nivelamento Automático da Mesa (BLTouch)

Após o flash do firmware, é importante calibrar o Z-offset e a malha de nivelamento da mesa:

1. **Resetar todas as configurações para o padrão**:
   ```
   M502 ; Resetar configurações
   M500 ; Salvar configurações
   M501 ; Carregar configurações salvas
   ```

2. **Fazer home em todos os eixos**:
   ```
   G28
   ```

3. **Mover para o centro da mesa**:
   ```
   G1 X107 Y107 Z5 F6000
   ```

4. **Abaixar o bico até que encoste em um pedaço de papel**:
   - Use pequenos movimentos no eixo Z (`G1 Z-0.05`, etc.) até sentir uma leve fricção no papel.

5. **Ler a posição atual e definir o Z-offset**:
   ```
   M114 ; Anote o valor do Z (ex: Z:-2.35)
   M851 Z-2.35 ; Substitua pelo seu valor
   M500 ; Salvar na EEPROM
   ```

6. **Gerar e salvar a malha de nivelamento da mesa**:
   ```
   G28    ; Fazer home novamente
   G29    ; Executar nivelamento automático da malha
   M500   ; Salvar malha na EEPROM
   ```

7. **Habilitar o nivelamento da malha em todas as impressões**:
   Certifique-se de que o G-code inicial do seu slicer inclua:
   ```
   G28
   M420 S1
   ```

💡 Se ainda não fez o flash do firmware, veja [Firmware Pré-compilado](#-firmware-pré-compilado) ou [Como Compilar](#-como-compilar).

## 🧰 Enviando Comandos via USB

Para enviar comandos G-code como `G28`, `G29` ou `M851`, você pode usar softwares gratuitos que se comunicam com sua impressora via cabo USB.

### Softwares Recomendados

- **Pronterface (Printrun)**  
  Download: [https://github.com/kliment/Printrun](https://github.com/kliment/Printrun)  
  Interface simples para enviar comandos e controlar a impressora manualmente.

- **OctoPrint**  
  Site: [https://octoprint.org](https://octoprint.org)  
  Interface web completa para controle remoto da impressora (requer Raspberry Pi ou computador).

- **Repetier-Host**  
  Site: [https://www.repetier.com/download-now/](https://www.repetier.com/download-now/)  
  Suporte para Windows/Linux/macOS com fácil conexão USB e console de comandos.

### Como Conectar via USB

1. Conecte sua Ender 3 ao computador via cabo USB (normalmente USB-B ou USB Mini-B).
2. Abra o software (ex: Pronterface).
3. Selecione a porta COM correta (geralmente detectada automaticamente).
4. Configure a taxa de transmissão para `115200`.
5. Clique em "Connect".
6. Uma vez conectado, você pode digitar comandos manualmente como:

Esses comandos são úteis para [calibração do Z-offset](#-calibração-do-z-offset-e-nivelamento-automático-da-mesa-bltouch).

   ```
   G28
   G29
   M114
   M851 Z-2.35
   M500
   ```

## 🧩 Como Compilar

1. **Baixe o Marlin 2.1.2.5**
   - Visite: [https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5](https://github.com/MarlinFirmware/Marlin/releases/tag/2.1.2.5)
   - Baixe o `Source code (zip)` e extraia.

2. **Copie os Arquivos de Configuração**
   - Clone ou baixe [este repositório](#configure-marlin-2125-for-ender-3-v1-with-creality-v427-board-and-bltouch).
   - Copie todos os arquivos (`Configuration.h`, `Configuration_adv.h`, `Version.h`, `_Bootscreen.h`, `_Statusscreen.h`) para a pasta `Marlin/` dentro da fonte extraída do Marlin.

3. **Abra o VSCode**
   - Abra a pasta raiz do Marlin no VSCode.
   - Instale a extensão [PlatformIO](https://platformio.org/platformio-ide) pela Marketplace do VSCode.
   - Instale a extensão [Auto Build Marlin](https://marketplace.visualstudio.com/items?itemName=MarlinFirmware.auto-build) pela Marketplace do VSCode.


4. **Compilar e Enviar**
   - Na barra lateral do VSCode, clique no ícone **Auto Build Marlin**.
   - Selecione:
     - **Environment**: `STM32F103RE_creality`
     - **Action**: Build ou Upload
   - Acompanhe o progresso e aguarde a confirmação.

5. **Flash do Firmware**
   - Após a compilação, o arquivo do firmware estará em:
     ```
     .pio/build/STM32F103RE_creality/firmware.bin
     ```
   - Copie o arquivo para a raiz de um **cartão microSD** (formatado como FAT32).
   - Desligue a impressora, insira o cartão e ligue novamente.
   - A impressora fará o flash automático do novo firmware e apagará o arquivo do cartão após concluir.


### 📚 Referência

- Para ver a configuração oficial padrão para Ender 3 (Marlin 2.1.2.5), visite:  
  [Configuração oficial Marlin Ender 3](https://github.com/MarlinFirmware/Configurations/tree/release-2.1.2.5/config/examples/Creality/Ender-3)

🔼 [Voltar ao topo](#configure-marlin-2125-para-ender-3-v1-com-placa-creality-v427-e-bltouch)