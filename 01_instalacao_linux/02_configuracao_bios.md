# Configuração da BIOS para Dual Boot (Windows + Linux)

Considere um computador com 2 SSDs, por exemplo: 
- SSD M.2 contendo o Windows
- SSD SATA contendo o Linux

Em muitos notebooks, principalmente com processadores Intel, é necessário alterar algumas configurações da BIOS para que o Linux funcione corretamente.

### Passo 1 — Entrar na BIOS

Ao ligar o computador:
- pressione `F2`, `DEL` ou a tecla correspondente da fabricante
- isso abrirá a BIOS/UEFI

### Passo 2 — Desbloquear opções avançadas da BIOS

Em alguns notebooks (principalmente Acer), a opção de alterar o modo do controlador SATA fica escondida. Sendo assim: 

Na aba:
```text
Main
```

pressione:
```text
Ctrl + S
```

Isso desbloqueia opções avançadas relacionadas ao armazenamento.

**Geralmente aparecerá algo como:**
```text
SATA Mode
```

ou:

```text
SATA Controller Mode
```

### Passo 3 — Alterar de RST para AHCI

Na nova opção desbloqueada, altere:

```text
Intel RST -> AHCI
```

---

#### OBS: Vamos entender
**O que é Intel RST?**

RST significa:

```text
Intel Rapid Storage Technology
```

É uma tecnologia da Intel usada para:
- RAID
- Intel Optane
- gerenciamento avançado de SSDs
- otimizações voltadas principalmente ao Windows

Quando o notebook está em modo RST:
- o Linux pode não detectar os SSDs
- o instalador pode falhar
- o GRUB pode não ser instalado corretamente

Isso acontece porque o controlador de armazenamento fica "escondido" atrás de drivers específicos da Intel.

**O que é AHCI?**

AHCI é um modo padrão e amplamente compatível para controladores SATA/NVMe.

O Linux possui suporte excelente para AHCI, então:
- os SSDs aparecem normalmente
- o kernel acessa os discos sem drivers especiais
- o dual boot funciona com muito menos problemas

Diante dessas questões, mudamos de RST para AHCI.

**Por que isso acontece mais em notebooks Intel?**

A tecnologia RST é da Intel.
Em computadores AMD:
- normalmente não existe Intel RST
- o Linux costuma detectar os SSDs diretamente
- o dual boot geralmente é mais simples

Mesmo assim:
- Secure Boot ainda pode causar problemas
- alguns notebooks AMD podem usar modos RAID próprios

---

### Passo 4 — Desativar Secure Boot
Na aba:
```text
Boot
```

desative:
```text
Secure Boot
```

Secure boot --> Disable

---

#### OBS: Vamos entender
**O que é Secure Boot?**

O Secure Boot é um mecanismo da UEFI que permite iniciar apenas sistemas assinados digitalmente.
O Windows, por exemplo, já vem preparado para isso.
Porém, dependendo da distribuição Linux e dos drivers utilizados:
- o GRUB pode não iniciar
- drivers NVIDIA podem falhar
- módulos do kernel podem ser bloqueados

Sendo assim, desativar o Secure Boot evita esses problemas durante o uso do Linux.

---

#### Observação importante sobre o Windows

Se o Windows foi instalado originalmente usando RST, mudar diretamente para AHCI pode causar erro de boot:

```text
INACCESSIBLE_BOOT_DEVICE
```

Isso acontece porque o Windows foi configurado para usar o driver RST.

Normalmente resolve:
1. ativando Safe Mode no Windows
2. alterando RST -> AHCI
3. iniciando o Windows em modo de segurança
4. reiniciando normalmente


#### Resumo

| Configuração | Motivo |
|---|---|
| `Ctrl + S` | desbloqueia opções ocultas da BIOS |
| `RST -> AHCI` | melhora compatibilidade do Linux |
| Desativar Secure Boot | evita problemas com GRUB e drivers |

---