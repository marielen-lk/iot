# iot

estacao_climatica.py

import dht
import machine
import time
import network
import urequests
from conexao_internet import conexao

d = dht.DHT11(machine.Pin(4))
r = machine.Pin(2, machine.Pin.OUT)
r.value(0)
contadorIf = contadorElse = contadorDados = 0

while True:
    d.measure()
    temperatura= (d.temperature())
    umidade = (d.humidity())
    print("Aluna: Marielen Latauczeski")
    print("Matricula: 123456789")
    print()
    print("Disciplina: Aplic. de Cloud, Iot e Indústria 4.0 em Python")
    print("Prof. Valmir Moro")
    print()
    print("Projeto: Estação Climática")
    print()
    print("A temperatura atual é: {}ºC.".format(temperatura))
    print("A umidade relativa do ar atual é: {}%.".format(umidade))
    print()
    print("Condições para o acionamento do relé: Temp. > 31ºC ou Umid. Rel. do Ar > 70%")
    print()
    time.sleep(4)
    if (temperatura >31 or umidade >70):
        r.value(1)
        contadorIf+=1
        print("Relé ligado.")
        print()
        print("Número de impressões no console: {}.".format(contadorIf))
        print()
        contadorElse = 0
    else:
        r.value(0)
        contadorElse+=1
        print("Relé desligado")
        print()
        print("Número de impressões no console: {}".format(contadorElse))
        print()
        contadorIf = 0
    print("Conexão com a rede Wi-Fi..")
    time.sleep(2)
    print("Conexão com a rede Wi-Fi realizada com sucesso!")
    print()
    #Nome do wi-fi e senha
    station = conexao("ESTACIO-VISITANTES", "estacio@2014")
    if not station.isconnected():
        print("Erro de conexão com a rede Wi-Fi! Tente novamente.")
        print()
    else:
        contadorDados +=1
        print("Acessando a plataforma ThingSpeak..")
        time.sleep(2)
        print()
        print("Acesso a plataforma ThingSpeak foi realizada com sucesso!")
        print()
        print("Enviando os dados...")
        response = urequests.get("https://api.thingspeak.com/update?api_key=5WRAORIQ6UNPJMF7&field1={}&field2={}".format(temperatura, umidade))
        time.sleep(2)
        print()
        print("Os dados foram enviados com sucesso!")
        print()
        print("{} coleta(s) de dado(s) enviado(s) a plataforma ThingSpeak".format(contadorDados))
        print()
        response.text
        station.disconnect()
        time.sleep(2)
