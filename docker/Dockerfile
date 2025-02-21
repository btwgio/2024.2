FROM python:3.12.1

# Diretorio de trabalho: /app
WORKDIR /app

# COPIA
COPY . /app/

# DEPENDENCIAS QUE A GENTE BAIXA MANUALMENTE SEMPRE NE
RUN pip install --upgrade pip && \
    pip install -r requirements.txt --verbose

# RODANDO
RUN python manage.py collectstatic --noinput

# EXPONDO A PORTA 8000
EXPOSE 8000

# COMANDOS 
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

#primeiro, crie os requerimentos: pip freeze > requirements.txt
#esse arquivo é o que o dockerfile usa para instalar as dependencias
#PARA RODAR USE:
#docker build --nocache -t rn-semfila . 
#PARA RODAR O CONTAINER:
# docker run -p 8000:8000 rn-semfila (PODE MUDAR DEPOIS ADICIONO)
#para rodar usando volumes:
#docker run -it -v /local:/app -p 8000:8000 rn-semfila
#quando vc usa volumes, o que vc altera no seu projeto, altera no container
#dessa forma acima vc tambem consegue acessar o terminal do container
#forma alternativa de rodar o container:
#docker run -it -v /local:/app -p 8000:8000 rn-semfila python manage.py runserver 
#dessa forma, vc consegue rodar o servidor do django no container diretamente



