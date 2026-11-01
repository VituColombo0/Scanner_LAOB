# Scanner_LAOB

Módulo de bancada para leitura, identificação e cadastramento de credenciais RFID (Mifare 13.56 MHz) para o ecossistema de controle de acesso do laboratório LAOB.

---

## Visão Geral

O **Scanner_LAOB** atua como estação de cadastro e validação rápida de tags e cartões RFID. Projetado para operar em conjunto com o sistema [Porta-LAOB](https://github.com/VituColombo0/Porta-LAOB), ele permite ler a UID (Unique Identifier) de crachás de novos membros, validar a integridade dos dados e exportar os registros formatados para o banco de credenciais do sistema de acesso.

### Principais Objetivos

- **Leitura de UID em Tempo Real:** Captura UIDs de 4 bytes (Mifare Classic 1K) e 7 bytes (Mifare Ultralight / NTAG) via SPI.
- **Formatação para o Porta-LAOB:** Converte leituras hexadecimais brutas diretamente para o formato esperado pelo gerenciador de usuários (`door_manager.py`).
- **Validação de Bancada:** Testa tags com defeito ou com sinal fraco antes da instalação física na porta do laboratório.
- **Interface Serial / CLI:** Exibição clara de status, tipo de cartão e código UID no terminal.

---

## Arquitetura de Integração

```
[ Cartão / Tag RFID ]
         │ (13.56 MHz RF)
         ▼
[ Leitor MFRC522 ] ──(SPI)──▶ [ Microcontrolador / Leitor USB ]
                                       │ (Serial / USB)
                                       ▼
                              [ Scanner_LAOB CLI ]
                                       │
                                       ▼
                       Exportação: authorized_tags.json
                                       │
                                       ▼
                              [ Porta-LAOB System ]
```

---

## Hardware e Conexões

O módulo utiliza o transceptor **MFRC522** conectado via barramento SPI a 3.3V.

| Pino MFRC522 | Função SPI | Nível Lógico | Observações |
| :--- | :--- | :--- | :--- |
| **VCC** | Alimentação | 3.3V | Não conectar a 5V sob risco de queima |
| **RST** | Reset | 3.3V | Reinicialização de hardware |
| **GND** | Terra | 0V | Referência de aterramento comum |
| **MISO** | SPI Master In Slave Out | 3.3V | Linha de recepção de dados |
| **MOSI** | SPI Master Out Slave In | 3.3V | Linha de envio de comandos |
| **SCK** | SPI Clock | 3.3V | Sinal de clock sincronizado |
| **SDA (SS)** | Slave Select / CS | 3.3V | Habilitação do dispositivo SPI |

---

## Pré-requisitos

- Python 3.10+ (se utilizado via interface de script/ponte serial no host)
- Leitor MFRC522 com pinagem soldada
- Microcontrolador compatível (Raspberry Pi Pico RP2040 ou Arduino)

---

## Fluxo de Operação

1. **Conexão:** Conecte o leitor à porta serial ou USB do computador de cadastro.
2. **Execução:** Inicie o monitor de leitura na taxa de 115200 bps.
3. **Aproximação:** Aproxime a nova tag do leitor até o sinal sonoro de confirmação.
4. **Registro:** Atribua o nome do aluno ou pesquisador ao código UID gerado.
5. **Sincronização:** Copie os dados cadastrados para a memória flash do dispositivo instalador no laboratório.

---

## Licença

Projeto desenvolvido para o Laboratório LAOB.
