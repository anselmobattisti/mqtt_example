# MQTT Example

# Installation

```shell
# create the venv environment
python3 -m venv venv

# access the venv
source venv/bin/activate

# install the packets
pip install -r requirements.txt

# leave the virtual environment
deactivate
```

# Softwares necessários:

1 - Python 3.8

2 - Uma conta free no hivehivemq (https://www.hivemq.com/)

# Como executar os testes:

1 - Rodar o Servidor Coap

```shell
# Parâmetros
# 1 - 192.168.0.113 ip do servidor coap
# 2 - 5683 porta

python server.py 192.168.0.113 5683
```

Dois recursos foram automaticamente criados, o /s1 e o /atuador

Aparecerá escrito "Add New Sensor", Digite o ID do sensor, por exmeplo s2

2 - Rode o atuador

```shell
python atuador.py 192.168.0.113 5683
```
O atuador usa pooling

Todos os leds devem ficar em branco

3 -Em um outro terminal rode o script que será a aplicação que sabe quais são os limites de um sensor específico e fará um post no /atuador que acenderá o led específico

```shell
# Parâmetros

# - s1 nome do recurso
# - 10 led que será acesso
# - 10 humidade
# - 10 temperatura
python app.py 192.168.0.113 5683 s1 10 10 10
```

O app usa observer

4 - Precisamos inserir um valor no recurso s1 do CoaP Server, abra outro terminar e execute 

```shell
python sensor_simulator.py -o PUT -p coap://192.168.0.113:5683/s1 -P 20-30

python sensor_simulator.py -o PUT -p coap://192.168.0.150:5683/s2 -P 10-30
```

Veja que o led 10 acendeu e o 20 ficou apagado pois a temperatura de s2 é 10. Se mudar a temp de s2 para 21 vai acender o led 20

```shell
python sensor_simulator.py -o PUT -p coap://192.168.0.150:5683/s2 -P 21-30
```