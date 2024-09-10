
# Flask Docker App

Este projeto demonstra como rodar uma aplicação Flask em um contêiner Docker, configurado em uma máquina virtual Ubuntu usando NAT no VMware Player.

Passo a Passo

# 0. Instalar o python e o pip na maquina

sudo apt install python3 python3-pip

# 1. Criar o Diretório do Projeto

mkdir dockertrab
cd dockertrab

# 2. Instalar o docker

sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce

# 3. Instalar o Flask

pip install Flask

# 4. Criar a aplicacao Flask

Dentro do diretório, crie um arquivo app.py com o seguinte código:

from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Olá, Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)

# 5. Criar requirements.txt e adicionar o Flask

touch requirements.txt
nano requirements.txt

Dentro do editor de texto

Flask==2.1.2
Werkzeug==2.0.3

# 6. Criar Dockerfile e adicionar ao Dockerfile

touch Dockerfile
nano Dockerfile

Dentro do editor de texto

FROM python:3.8-slim
WORKDIR /app
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8080
CMD ["python3", "app.py"]

# 7. Criar ambiente virtual

python3 -m venv venv
source venv/bin/activate

# 8. Construir a imagem do docker e rodar o container

sudo docker build -t flask-docker-app .
sudo docker run -d -p 8080:8080 flask-docker-app

# 9. Verificar o funcionamento

sudo docker ps

