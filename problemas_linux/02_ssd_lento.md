# Diagnóstico de SSD SATA com SMART, benchmark e TRIM

Às vezes, o Linux pode apresentar lentidão severa ou travamentos, especialmente em situações de alta pressão de memória. <br>
Quando muitos processos passam a consumir RAM ao mesmo tempo, a memória disponível pode se esgotar e o sistema começa a utilizar a área de SWAP. Se essa SWAP estiver localizada em um SSD que, naquele momento, apresenta baixa taxa de leitura ou escrita e alta latência de I/O, o problema pode se agravar significativamente, fazendo com que aplicações parem de responder e o sistema inteiro fique muito lento. <br>
Em casos mais extremos, um travamento ou reinicialização forçada pode fazer com que o sistema execute uma verificação automática do sistema de arquivos no próximo boot. Após esse tipo de situação, também pode ser útil verificar se o SSD está gerenciando corretamente os blocos livres por meio do TRIM, já que uma degradação temporária no desempenho do armazenamento pode estar relacionada ao gerenciamento interno de blocos, garbage collection e operações de descarte.

Este procedimento tem como objetivo verificar a saúde de um SSD SATA, medir sua velocidade de escrita, executar o TRIM manualmente e repetir o benchmark para comparar o desempenho antes e depois.


## 1. Identificar o SSD

Primeiro, liste os discos:
```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,MODEL
```

Exemplo:
```text
sda      223,6G disk                    Patriot Burst Elite 240GB
├─sda1     7,6G part swap               [SWAP]
├─sda2   214,9G part ext4               /
└─sda3       1G part vfat               /boot/efi
```

Nesse exemplo, o SSD é:

```text
/dev/sda
```


## 2. Verificar a saúde do SSD com SMART

Se `smartctl` não estiver instalado:
```bash
sudo apt update
sudo apt install smartmontools
```

Verificação completa:
```bash
sudo smartctl -a /dev/sda
```

Os principais campos para observar são:

```text
SMART overall-health self-assessment test result
Reallocated_Sector_Ct
Current_Pending_Sector
Runtime_Bad_Block
End-to-End_Error
UDMA_CRC_Error_Count
Temperature_Celsius
SMART Error Log
```

Indicadores especialmente preocupantes incluem:

```text
Current_Pending_Sector > 0
Runtime_Bad_Block > 0
UDMA_CRC_Error_Count crescendo
SMART Error Log contendo erros
SMART overall-health = FAILED
```

> **Observação:** alguns SSDs possuem atributos SMART específicos do fabricante. <br> 
Se aparecer:
> ```text
> Device is: Not in smartctl database
> ```

nem todo `RAW_VALUE` pode ser interpretado literalmente.


## 3. Executar um teste SMART 

### SMART curto
```bash
sudo smartctl -t short /dev/sda
```

O comando informa quanto tempo o teste deve levar. <br>
Após esse tempo, consulte novamente:
```bash
sudo smartctl -a /dev/sda
```

Procure por:
```text
SMART Self-test log
```

Um resultado normal é:
```text
Short offline    Completed without error
```


### SMART completo
Para uma análise mais profunda:
```bash
sudo smartctl -t long /dev/sda
```

O próprio SMART informa a duração prevista. <br>
Depois:
```bash
sudo smartctl -a /dev/sda
```

O teste completo é mais demorado, mas verifica uma área muito maior do SSD.


## 4. Procurar erros SATA ou de I/O no kernel
```bash
sudo dmesg -T | grep -iE "sda|ata|ext4|i/o|error|fail|reset|timeout"
```

Mensagens preocupantes seriam, por exemplo:
```text
I/O error
Buffer I/O error
ata1: hard resetting link
COMRESET failed
failed command
EXT4-fs error
```

Para consultar registros do boot atual:
```bash
sudo journalctl -k | grep -iE "sda|ata|I/O|timeout|reset|failed|error"
```


## 5. Verificar espaço livre

```bash
df -h /
```

Exemplo:
```text
Sist. Arq.   Tam. Usado Disp. Uso%
/dev/sda2    211G  161G   40G  81%
```

SSD muito cheio pode perder desempenho, principalmente modelos de entrada.


## 6. Medir velocidade de escrita do SSD

Não usar `dd` diretamente em `/dev/sda`, pois isso destruiria dados.

Crie um arquivo de teste no filesystem:
```bash
sync
dd if=/dev/zero of=~/ssd_test.bin bs=1G count=5 oflag=direct status=progress
```

Esse comando grava 5 GiB diretamente no armazenamento, evitando boa parte do cache do sistema operacional. 

No final será mostrado algo como:
```text
5368709120 bytes copied, 10.48 s, 512 MB/s
```

A velocidade de escrita é o último valor:
```text
512 MB/s
```

Depois do teste:
```bash
rm -rf ~/ssd_test.bin
```

No caso diagnosticado, antes do TRIM foi observado:
```text
7.5 MB/s
```

Depois do TRIM:
```text
512 MB/s
```


## 7. Medir velocidade de leitura
Uma opção simples:
```bash
sudo hdparm -Tt /dev/sda
```

A linha mais importante é:
```text
Timing buffered disk reads
```

Ela representa uma estimativa da leitura sequencial do SSD.


## 8. Verificar se TRIM é suportado

```bash
lsblk -D
```

Para o SSD, campos como:
```text
DISC-GRAN
DISC-MAX
```

diferentes de zero indicam suporte a discard/TRIM.


## 9. Verificar o TRIM automático
```bash
systemctl status fstrim.timer
```

Em sistemas Ubuntu normalmente deve aparecer:
```text
Active: active (waiting)
```

O timer normalmente executa o TRIM periodicamente.

Para ver execuções anteriores:
```bash
journalctl -u fstrim.service
```


## 10. Executar TRIM manualmente

Para o filesystem raiz:
```bash
sudo fstrim -v /
```

Exemplo:
```text
/: 50 GiB descartado
```

Isso informa ao SSD quais blocos do filesystem não estão mais sendo utilizados.

Importante: o número mostrado pelo `fstrim` representa a quantidade de espaço submetida para descarte. Ele não significa necessariamente que havia exatamente aquela quantidade de blocos "problemáticos".


## 11. Repetir o benchmark depois do TRIM
Depois do:

```bash
sudo fstrim -v /
```

execute novamente:
```bash
sync
dd if=/dev/zero of=~/ssd_test_after_trim.bin bs=1G count=5 oflag=direct status=progress
```

Depois remova:

```bash
rm -rf ~/ssd_test_after_trim.bin
```

Compare:
```text
ANTES DO TRIM:
7.5 MB/s

DEPOIS DO TRIM:
512 MB/s
```

Uma diferença tão grande indica que o SSD estava sofrendo algum tipo de degradação temporária de desempenho associada ao gerenciamento interno de blocos, garbage collection, discard ou à carga existente naquele momento.


## 12. Observar o SSD durante travamentos

Para analisar latência e saturação:
```bash
iostat -xz 1
```

Os campos mais importantes são:
```text
r_await
w_await
aqu-sz
%util
```

Valores como:
```text
%util ≈ 90–100%
r_await = dezenas ou centenas de ms
aqu-sz elevado
```

indicam que o armazenamento está congestionado.

Também observe o uso da CPU:
```text
%iowait
```

Valores elevados de `iowait`, como 30%, 50% ou mais, significam que a CPU está frequentemente esperando operações de armazenamento.


## 13. Descobrir qual processo está usando o disco

Instale:
```bash
sudo apt install iotop
```

Depois:
```bash
sudo iotop -oPa
```

Isso ajuda a identificar processos responsáveis por leitura e escrita excessivas.


## Procedimento resumido

Quando o computador começar a travar novamente:

```bash
sudo smartctl -a /dev/sda
```

```bash
sudo dmesg -T | grep -iE "sda|ata|ext4|i/o|error|fail|reset|timeout"
```

```bash
iostat -xz 1
```

Teste de velocidade:

```bash
sync
dd if=/dev/zero of=~/ssd_test.bin bs=1G count=5 oflag=direct status=progress
```

Remova:

```bash
rm ~/ssd_test.bin
```

Execute TRIM:

```bash
sudo fstrim -v /
```

Teste novamente:

```bash
sync
dd if=/dev/zero of=~/ssd_test_after_trim.bin bs=1G count=5 oflag=direct status=progress
```

Remova:

```bash
rm ~/ssd_test_after_trim.bin
```

Compare as velocidades antes e depois.
