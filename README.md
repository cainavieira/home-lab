# 🖥️ Home Lab

Documentação do meu laboratório pessoal de infraestrutura. 

---

## 🎯 Objetivo Final

Transformar uma máquina Dell em um servidor Proxmox VE capaz de rodar máquinas virtuais e containers, aprendendo Linux, redes e administração de sistemas na prática.

---

## 🔧 Hardware

| Componente | Descrição | 
|---|---| 
| Máquina | Dell (hardware legado) |
| CPU | Intel Core i3-550 @ 3.20GHz — 4 cores, 1 socket |
| RAM | 1.8 GB |
| Virtualização | VT-x ativo  |
| Disco principal | SSD ~112GB — sistema operacional e hypervisor |
| Disco secundário | HD ~298GB — futuro storage de ISOs e backups |

---

## 🗺️ Stack de Software

| Camada | Tecnologia |
|---|---|
| Sistema base | Debian 13 (Trixie) |
| Kernel | 6.12.73+deb13-amd64 |
| Hypervisor | Proxmox VE (em instalação) |
| Acesso remoto | SSH (pendente pois estou sem rede ainda) |

---

## 📊 Status Atual

| Etapa | Status |
|---|---|
| Instalação do Debian 13 | ✅ Concluído |
| Configuração de teclado (pt-BR) | ✅ Concluído |
| Drivers da placa de rede | ✅ Concluído |
| IP estático configurado | ✅ Concluído |
| Acesso remoto via SSH | ✅ Concluído |
| Instalação do Proxmox VE | ✅ Concluído  |
| Primeiro LXC Container | ⏳ Pendente |
| HD secundário montado como storage | ⏳ Pendente |

---

## 📁 Estrutura do Repositório
```
homelab/
│
├── README.md                        ← Explicação do escopo do projeto
│
├── docs/                            ← diários técnicos por fase
│   ├── semana-01-instalacao-debian.md
│   └── semana-02-configuracao-rede.md
│   └── semana-03-instalacao-promox.md
│
├── configs/                         ← cópias dos arquivos de configuração do servidor
│   └── (arquivos adicionados conforme o lab evolui)
│
└── diagramas-e-infra/                       ← topologia de rede e infraestrutura
    └── (imagens adicionadas conforme o lab evolui)
```
---

## 📓 Diário de Progresso

| Semana | Tema | Link |
|---|---|---|
| Semana 01 | Instalação do Debian 13 em hardware legado | [ver doc](docs/semana-01-instalacao-debian.md) |
| Semana 02 | Configuração de rede, repositórios e acesso SSH | [ver doc](docs/semana-02-configuracao-rede.md) |
| Semana 03 | Instalação do Promox 9.1 em cima do debian 13 | [ver doc](docs/semana-03-instalacao-promox.md) |

## 🧠 Estudo que estou fazendo em paralelo

- Linux (administração de sistemas, CLI)
- Redes — Cisco (12ª maratona)
- Proxmox VE e virtualização

---

## 💡 Por que estou fazendo isso

Aprender infraestrutura de verdade exige praticar, quebrar coisas e resolver problemas reais. Esse repositório documenta os estudos teóricos que estou colocando em prática e representa meu desenvolvimento como profissional na área de tecnologia.
