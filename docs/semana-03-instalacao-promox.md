# Documentação do Home Lab – Semana 3

## Resumo

Semana de transição. O Debian estava funcionando, SSH configurado, base pronta. O objetivo agora era instalar o Proxmox VE por cima. Segui a documentação oficial para instalação no Debian 13 Trixie, disponível [aqui](https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_13_Trixie).  
Não vou reescrever o que o tutorial já explica bem.   
O que vale documentar é o que aprendi no processo e as decisões que tomei dado o meu hardware.

---

## 1. O /etc/hosts e Por Que Isso Importa

O primeiro passo do tutorial antes de instalar qualquer coisa foi garantir que o hostname do servidor resolvesse para o IP real da máquina, não para um endereço de loopback.

Isso não é burocracia. O sistema de cluster do Proxmox precisa encontrar o servidor pelo IP real da interface de rede. Se o hostname `pve` resolver para `127.0.1.1` em vez de `192.168.0.100`, o Proxmox vai tentar se comunicar internamente pelo loopback e falhar. O arquivo ficou assim:

```
127.0.0.1       localhost
192.168.0.100   pve
```

Primeira vez que editei um arquivo de sistema como root e entendi o porquê de cada linha.

---

## 2. Repositórios no Formato deb822

Para instalar o Proxmox, o `apt` precisa saber onde buscar os pacotes. O tutorial usa o formato moderno de repositórios do Debian chamado deb822, que são blocos legíveis de chave e valor, diferente das linhas longas do formato antigo do `sources.list`.

Depois de adicionar o repositório, baixei a chave GPG oficial e verifiquei a integridade com `sha256sum` e `md5sum`. Primeira vez que usei verificação de checksum na prática o conceito faz sentido: garantir que o arquivo baixado é exatamente o que o servidor entregou, sem adulteração.

---

## 3. Troca de Kernel

O Proxmox exige seu próprio kernel. Não é opcional pois alguns pacotes dependem de flags de compilação e extensões específicas que o kernel padrão do Debian não tem.

Instalei o `proxmox-default-kernel`, dei reboot e confirmei com `uname -r` que o sufixo `-pve` estava ativo. Depois disso, removi os kernels originais do Debian. Manter os dois criaria conflito em atualizações futuras. Atualizei o GRUB para refletir a mudança e removi o `os-prober`. Esse pacote escaneia os discos em busca de outros sistemas operacionais para criar entradas de boot duplo, o que no futuro causaria problemas ao varrer os discos virtuais das VMs e containers.

---

## 4. Instalação e o Diagnóstico de RAM

Com o kernel certo rodando, a instalação em si foi direta. O pacote principal puxa junto o `postfix`, o `open-iscsi` e o `chrony`. O `chrony` cuida da sincronização de tempo via NTP e no Proxmox isso é crítico porque os logs de auditoria e a comunicação interna do cluster dependem de relógios precisos.

Após a instalação, acessei a interface web em `https://192.168.0.100:8006`.

O painel trouxe um diagnóstico imediato: **1.79 GiB de RAM total, com cerca de 64% já em uso só para manter os serviços do Proxmox ativos após o boot**.

Isso mudou a estratégia do laboratório. VMs tradicionais estão fora por enquanto, porque cada uma consumiria centenas de megabytes só para inicializar. O foco vai ser inteiramente em **containers**,  pelo menos por agora.

---

## 5. Linux Bridge (vmbr0)

Para que os containers consigam acessar a rede, o Proxmox precisa de uma ponte virtual conectando a interface física do servidor ao ambiente virtualizado.

Criei a `vmbr0` pela interface gráfica, apontando para a `enp2s0` com o IP e gateway já configurados. A bridge funciona como um switch virtual: os containers se conectam nela como se fossem dispositivos na mesma rede física, e o tráfego sai pela interface real do servidor.

---

## Status no Final da Semana

Proxmox VE instalado e acessível pelo navegador. Interface web funcionando, bridge de rede configurada. Hardware limitado mas operacional. O laboratório tem um hypervisor rodando.

---

## Próximos Passos

O Proxmox está instalado mas ainda é território desconhecido. O próximo período vai ser de exploração da interface, entender o que é possível fazer com o hardware disponível e criar o primeiro container LXC. O caminho vai se definindo conforme o laboratório evolui.