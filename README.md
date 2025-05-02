# Clientes de Rede UDP e Raw Socket

Este repositório contém dois scripts Python que implementam clientes de rede para interagir com um servidor específico (`15.228.191.109` na porta `0xC350` - 50000). Ambos os clientes enviam requisições para obter informações como data/hora, mensagens motivacionais ou a contagem de respostas do servidor, mas utilizam abordagens diferentes para a comunicação de rede.

## 📜 Scripts

### 1. `udp.py` - Cliente UDP Padrão

Este script implementa um cliente que utiliza sockets UDP padrão (`socket.SOCK_DGRAM`) para se comunicar com o servidor. O sistema operacional é responsável por construir e gerenciar os cabeçalhos UDP.

**Funcionalidades:**

* Conecta-se ao servidor especificado via UDP.
* Apresenta um menu com opções para o utilizador:
    * 1: Solicitar data e hora atual.
    * 2: Solicitar mensagem motivacional.
    * 3: Solicitar contagem de respostas do servidor.
    * 4: Sair.
* Envia requisições formatadas para o servidor com um identificador aleatório.
* Recebe a resposta do servidor.
* Descodifica e exibe a resposta recebida (string ou inteiro).

**Como Executar:**

1.  Certifique-se de ter o Python instalado.
2.  Execute o script a partir do terminal:
    ```bash
    python udp.py
    ```
3.  Siga as instruções no menu para enviar requisições.

### 2. `raw.py` - Cliente Raw Socket

Este script implementa um cliente que utiliza sockets RAW (`socket.SOCK_RAW` com `IPPROTO_UDP`). Diferente do cliente UDP padrão, este script constrói manualmente o cabeçalho UDP e calcula o checksum necessário antes de enviar o pacote.

**Funcionalidades:**

* Cria um socket RAW para enviar pacotes UDP.
* Constrói manualmente o cabeçalho UDP, incluindo portas de origem/destino, tamanho e checksum.
* Calcula o checksum UDP utilizando um pseudo-cabeçalho (IPs de origem/destino, protocolo, tamanho UDP).
* Apresenta o mesmo menu de opções do cliente `udp.py`.
* Envia as requisições formatadas (com cabeçalho UDP manual) para o servidor.
* Recebe o pacote IP completo de resposta do servidor.
* Extrai a carga útil (payload) UDP do pacote IP recebido.
* Descodifica e exibe a resposta contida na carga útil.

**Como Executar:**

1.  Certifique-se de ter o Python instalado.
2.  **Importante:** A execução de scripts que utilizam sockets RAW geralmente requer privilégios de administrador (root no Linux/macOS, "Executar como administrador" no Windows).
3.  Execute o script a partir do terminal com os privilégios necessários:
    * Linux/macOS: `sudo python raw.py`
    * Windows: Abra o terminal como Administrador e execute `python raw.py`
4.  Siga as instruções no menu para enviar requisições.

## ⚙️ Protocolo de Comunicação (Simplificado)

* **Requisição (Cliente -> Servidor):**
    * Byte 1: Tipo da requisição (0x00 para data, 0x01 para mensagem, 0x02 para contagem).
    * Bytes 2-3: Identificador aleatório (16 bits, big-endian).
* **Resposta (Servidor -> Cliente):**
    * Byte 1: Tipo da resposta (0x10, 0x11, 0x12 correspondendo aos tipos de requisição).
    * Bytes 2-3: Identificador da requisição original.
    * Byte 4: Tamanho da carga útil da resposta (N bytes).
    * Bytes 5 a 5+N-1: Dados da resposta (string ou inteiro).

## 🖥️ Servidor Alvo

* **IP:** `15.228.191.109`
* **Porta:** `0xC350` (Decimal: 50000)

## 📦 Dependências

Ambos os scripts utilizam apenas bibliotecas padrão do Python:

* `socket`
* `random`
* `types`
