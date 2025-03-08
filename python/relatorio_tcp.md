# Tutorial: Criando um Servidor TCP Simples em Python

## Sumário
1. [Socket Server](#socket-server)
2. [Socket Cliente](#socket-cliente)

## 1. Socket Server

Esta parte do tutorial explica como criar um servidor TCP simples em Python usando o módulo `socketserver`. O servidor recebe dados de um cliente, imprime os dados recebidos no console e envia de volta os dados em letras maiúsculas.

### Passo 1: Importar o Módulo `socketserver`

```python
import socketserver
```

### Passo 2: Criar a Classe Manipuladora de Requisições

```python
class MyTCPHandler(socketserver.BaseRequestHandler):
    def handle(self):
        self.data = self.request.recv(1024).strip()
        print("Recebido de {}:".format(self.client_address[0]))
        print(self.data)
        self.request.sendall(self.data.upper())
```

### Passo 3: Configurar e Iniciar o Servidor

```python
if __name__ == "__main__":
    HOST, PORT = "10.25.2.25", 80
    with socketserver.TCPServer((HOST, PORT), MyTCPHandler) as server:
        server.serve_forever()
```

## 2. Socket Cliente

Esta parte do tutorial explica como criar um cliente socket TCP em Python que se conecta a um servidor, envia dados e recebe uma resposta.

### Passo 1: Importar os Módulos Necessários

```python
import socket
import sys
```

### Passo 2: Definir o Endereço do Servidor e os Dados a Serem Enviados

```python
HOST, PORT = "10.25.2.25", 80
data = " ".join(sys.argv[1:])
```

### Passo 3: Criar o Socket e Enviar Dados

```python
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
    sock.connect((HOST, PORT))
    sock.sendall(bytes(data + "\n", "utf-8"))
    received = str(sock.recv(1024), "utf-8")

print("Sent:     {}".format(data))
print("Received: {}".format(received))
```
