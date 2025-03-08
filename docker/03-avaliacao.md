# Docker compose com Django e banco de dados

## Informações gerais

- Assunto: Docker, conteinizar aplicativos
- **Tarefa**:
  1. Criar um projeto django com uma aplicação web
    - alternativa, criar uma _branch_ no repositório do projeto integrador para configurar o acesso ao repositório de dados
  2. Criar um `Dockerfile` para o projeto django
  3. Criar a imagem e testar o conteiner para testar
  4. OPCIONAL, porque dependendo de como pergunta ao assistente de IA; criar um `Dockerfile` para o repositório de banco de dados
  5. Criar um `docker-compose.yml` e configurar para 2 serviços: `webapp` e `db`
  6. Configurar o arquivo django de acesso ao repositório de dados para usar o serviço docker `db`
  7. Testar o `docker-compose.yml`
  8. Relatar minimamente o que foi feito.

## Relatório

### Aluno

- Nome: Giovanna Gabrielle Albuquerque de Moura
- Matrícula: 20232014040013

### Relato do Dockerfile
1. Estrutura do Dockerfile:

```
FROM python:3.12.1

WORKDIR /app


COPY . /app/


RUN pip install --upgrade pip && \
    pip install -r requirements.txt --verbose

    
RUN python manage.py collectstatic --noinput


EXPOSE 8000


CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```
Explicação:

- FROM python:3.12.1: define a imagem base como Python 3.12.1.

- WORKDIR /app: define o diretório de trabalho dentro do container como /app.

- COPY . /app/: copia todos os arquivos do diretório local para o diretório /app no container.

- RUN pip install --upgrade pip && pip install -r requirements.txt --verbose: atualiza o pip e instala as dependências listadas no arquivo requirements.txt.

- RUN python manage.py collectstatic --noinput: coleta os arquivos estáticos do projeto Django.

- EXPOSE 8000: expõe a porta 8000 do container.

- CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]: executa o servidor Django quando o container é iniciado.

2. Criação do arquivo requirements.txt

Antes de construir a imagem Docker, é necessário criar o arquivo requirements.txt com as dependências do projeto. Utilize o seguinte comando:
```
pip freeze > requirements.txt
```
3. Build da imagem Docker
```
docker build --no-cache -t rn-semfila .
```
- --no-cache: garante que a imagem seja construída do zero, sem utilizar o cache.
- -t rn-semfila: define o nome da imagem como "rn-semfila".

4. Execução do container Docker

Existem diferentes formas de executar o container:

4.1 Execução simples:
```
docker run -p 8000:8000 rn-semfila
```
- -p 8000:8000: mapeia a porta 8000 do host para a porta 8000 do container.

4.2 Execução com volumes:
```
docker run -it -v /local:/app -p 8000:8000 rn-semfila
```
- -it: permite a interação com o terminal do container.
- -v /local:/app: monta o diretório /local do host no diretório /app do container. Alterações nos arquivos do host serão refletidas no container.

4.3 Execução direta:
```
docker run -it -v /local:/app -p 8000:8000 rn-semfila python manage.py runserver
```
- Permite executar o servidor Django diretamente no container, com volumes montados.

### Relato do Docker Compose

1. Estrutura do Docker Compose
   
- É necessário criar um arquivo chamado docker-compose.yml no mesmo diretório do Dockerfile
```
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - .:/app
    command: python manage.py runserver 0.0.0.0:8000
    depends_on:
      - db
    environment:
      - DATABASE_URL=${DATABASE_URL}
  db:
    image: postgres:13
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
    volumes:
      - postgres_data:/var/lib/postgresql/data/
volumes:
  postgres_data: 
```
2. Executando o Docker Compose
   
```
docker-compose up --build
```
- docker-compose up: inicia os serviços definidos no arquivo docker-compose.yml.
- --build: constrói a imagem se ela não existir ou se o Dockerfile tiver sido alterado.
