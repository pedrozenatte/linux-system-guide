# Problema com o Wifi e a economia de energia

A rede de internet fica instável e só é percebida em chamadas, por exemplo, pelo meet ou pelo slack. 
Esse erro, vem por conta que a economia de energia do Wi-Fi é um modo em que o Linux tenta reduzir o consumo da placa de rede sem fio.
Em vez de deixar a placa Wi-Fi sempre totalmente “acordada”, o sistema permite que ela entre em pequenos estados de repouso quando acha que não está usando muito a rede. Isso ajuda a economizar bateria, mas pode causar microatrasos ou pequenas perdas de estabilidade.

##### Desligando de forma não-permanente
```bash
pedro@pedro-Aspire-A515-54G:~$ iw dev wlp4s0 get power_save
Power save: on
pedro@pedro-Aspire-A515-54G:~$ sudo iw dev wlp4s0 set power_save off
pedro@pedro-Aspire-A515-54G:~$ iw dev wlp4s0 get power_save
Power save: off
```
**OBS:** Se reiniciar o PC ou reconectar no wifi, volta nas configurações default. 

##### Para desligar permanentemente o power save do Wi-Fi, faça pelo NetworkManager:
```bash
sudo nano /etc/NetworkManager/conf.d/wifi-powersave.conf
```

E coloque:
```text
[connection]
wifi.powersave = 2
```
Isso desativa a economia de energia do Wi-Fi nas conexões gerenciadas pelo NetworkManager.

Depois reinicie o NetworkManager:
```bash
sudo systemctl restart NetworkManager
```

Teste com o comando:
```bash
iw dev wlp4s0 get power_save
```