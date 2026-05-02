# Instalação Linux
A instalação pode ser feita seguindo o guia oficial da documentação do Ubuntu: https://documentation-ubuntu-com.translate.goog/desktop/en/latest/tutorial/install-ubuntu-desktop/?_x_tr_sl=en&_x_tr_tl=pt&_x_tr_hl=pt&_x_tr_pto=tc. 
Em caso de insegurança, é bom dar uma revisitada. 

## Pendrive boot
O primeiro passo é baixar a imagem ISO do sistema Linux.  
Normalmente utilizo o Ubuntu 20.04 LTS (Focal Fossa), pois é a última versão LTS compatível oficialmente com o ROS 1 Noetic.
Após baixar a ISO, precisamos criar um pendrive bootável.
- No Windows, costumo utilizar o software Rufus.
- No próprio Linux, existem outras opções recomendadas no guia oficial do Ubuntu.

O processo consiste basicamente em selecionar:
1. O pendrive.
2. A ISO do Ubuntu.
3. Iniciar a gravação.

OBS: O sistema de gravação de dados pode deixar em 4KB. 

## Configurar a bios
Após criar o pendrive bootável, reinicie o computador e acesse a BIOS/UEFI.
Na aba de boot, coloque o pendrive como primeira opção de inicialização, pois, assim, o computador carregará o sistema diretamente pelo pendrive, permitindo iniciar a instalação do SO.


## Instalador 
Com o sistema carregado pelo pendrive, selecione a opção **Instalar Ubuntu** e siga as etapas básicas do instalador:
- Escolha de idioma.
- Layout do teclado.
- Conexão com a internet.
- Configurações regionais.
- Usuário e senha.

Além disso, utilize a instalação **INTERATIVA** e marque a opção para para **instalar softwares de terceiros**. Essa opção é importante para instalar drivers proprietários, como drivers de placas de vídeo NVIDIA, codecs e outros componentes adicionais.

### Configurações do disco
Vamos selecionar a instalação **MANUAL**. É aqui que começa a esquecer das configurações.
Como gosto de manter um SSD separado para cada sistema operacional e preservar o funcionamento correto do GRUB, costumo configurar da seguinte forma: 

#### 1) Vamos criar duas partições para o linux:
1) Partição de SWAP: Vamos manter o **mesmo tamanho de memória RAM**.

2) Partição principal do sistema
    - Sistema de arquivos: **EXT4**
    - Ponto de montagem: `/`
**tamanho será de todo o resto do SSD**. 

OBS: Se quiser também pode colocar uma terceria partição /home para preservar documentos, assim, em caso de formatação será preservado. 

#### 2) Configurar o GRUB
Por questões de compatibilidade, vamos **colocar o dispositivo no qual será instalado o carregador de inicialização do linux no SSD que contém o Windows, no meu caso: /dev/nvme0n1**.
OBS: Cuidado para não colocar no nvme0n1p1, pois isso é uma partição. 

**SÓ INSTALAR**