<div align="center">

<img width="256" height="256" alt="kali-medusa-2617811567" src="https://github.com/user-attachments/assets/0de06e87-fb2a-47ea-a5f6-8e8fac768e05" />

# Medusa bruteforce lab
### Simulação controlada de brute force (FTP,DVWA e SMB) com Medusa e Kali Linux.
[![Security](https://img.shields.io/badge/Security-Penetration%20Testing-red.svg)](https://github.com/0x4m4/hexstrike-ai)

</div>

O ataque consiste em um laboratório controlado (Metasploitable 2), intencionalmente vulnerável a ataques de força bruta, enumeração, exploração e Pós exploração, o ambiente esta isolado da internet, apenas em uma rede interna que conecta 2 maquinas isoladas para a atividade.

Usaremos a seguintes ferramentas para essa atividade:

- **Medusa**

- **Enum4linux**

- **Nmap**

- **Ping** (Antes de qualquer coisa, precisa verificar se tem conectividade com o alvo.)

- **Word_lists** (Fique a vontade para usar as word_lists desse repositório ou os seus próprios, a experiencia de criar os seus próprios aumenta na experiencia da atividade.)


#### Caso queira as ferramentas em suas distros próprias, logo abaixo estará suas instalações, mas na distro Kali Linux, já vem com todas essas ferramentas pré-instaladas.

```bash
# 1. Atualização dos pacotes
sudo apt update
sudo apt upgrade

# 2. Instalação da medusa
sudo apt install medusa

# 3. Instalação do enum4linux
sudo apt install enum4linux

# 4. Instalação do nmap
sudo apt install nmap
```
