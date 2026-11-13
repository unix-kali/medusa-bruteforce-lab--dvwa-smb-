<div align="center">

<img width="256" height="256" alt="kali-medusa-2617811567" src="https://github.com/user-attachments/assets/0de06e87-fb2a-47ea-a5f6-8e8fac768e05" />

# Medusa bruteforce lab
### Simulação controlada de brute force (FTP e SMB) com Medusa e Kali Linux.
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
### host-only
<img width="701" height="289" alt="acedab06-5f84-4a2a-ad8e-c419042e4dee" src="https://github.com/user-attachments/assets/7e0a61e1-00d3-4f94-be72-cc1ced373c7a" />

Após você configurar nas duas maquinas como está na imagem, você terá que configurar o endereço IPv4 nas duas máquinas como na próxima imagem, para que possam se comunicar.

```bash
sudo ip addr add 192.168.0.10/24 dev eth0 #Endereço da maquina alvo

sudo ip addr add 192.168.0.11/24 dev eth0 #Endereço do Kali
```

Na sua máquina Kali Linux, teste a comunicação usando o ping no IP alvo.
```bash
ping -c 192.168.0.10
```

<img width="858" height="379" alt="e8699394-cea8-4cc5-be85-c07f197b70c1" src="https://github.com/user-attachments/assets/a7cb44e2-4d22-4489-86e8-647811b38d97" />

- ## FTP
Protocolo de Transferência de Arquivos).
Após se certificar que a comunicação entre as 2 máquinas, faremos uma varredura da maquina alvo, normalmente não faremos scans muito agressivos, mas nesse caso estamos em um laboratório protegido sem nenhum escopo ou vigilância impedindo.
Usaremos o nmap para fazer uma varredura de portas,serviços e protocolos do alvo.

### nmap

Aqui vai um resumo da ferramenta nmap, caso já saiba da ferramenta pode passar.

O Nmap (Network Mapper) é um utilitário livre e de código aberto, focado em descoberta de redes, auditoria de segurança e inventário de sistemas.
Ele funciona como uma ferramenta de pacotes e protocolos que buscam trazer o máximo de informações possíveis, mas o controle depende unicamente do conhecimento do Operador, atuando em um nível bastante próximo aos pacotes e protocolos de rede (TCP, UDP, ICMP, ARP), manipulando cabeçalhos e analisando as respostas cruas que os sistemas devolvem.

Digite com esses parâmetros:

```bash
nmap -sV -O 192.168.0.10
```
Essa é a visão que você deverá ter:

<img width="910" height="612" alt="2aabb971-743c-45f8-86c6-15c30bbda7e1" src="https://github.com/user-attachments/assets/b3661f88-c765-4f45-81ad-761f5afe580b" />

mais informações: [https://nmap.org/man/pt_BR/index.html](https://nmap.org/man/pt_BR/index.html)

### enum4linux

Também precisamos ver se conseguimos saber quem são os usuários, isso irá melhorar a precisão do nosso ataque, utilizaremos o "Enum4linux" Essa ferramenta automatiza a extração de informações de sistemas Windows e Samba (Linux) usando exatamente o protocolo SMB (que pode ser vista no comando do nmap com protocolo Netbios-ssn 139).

-Explicação rápida:
(Netbios-ssn É um protocolo de sessão legado (criado nos anos 80) que permitia que computadores se encontrassem por nomes (ex: PC-SALA) e abrissem uma sessão de comunicação.)

```bash
enum4linux -a 192.168.0.10  #Caso sua maquina esteja com esse ip.
```
Seu objetivo é encontrar essa parte das informações do sistema:
<img width="907" height="647" alt="ed96a835-42fb-42ac-949d-1c58f46144a3" src="https://github.com/user-attachments/assets/b7a25a68-b247-4b6d-ae3b-27714863cdab" />

Nesse repositório criei a minha própria word-list, usando alguns usuários desse grupo, se sinta a vontade caso queira criar o seu próprio.

- ## Exploração

A partir de agora, o que deverá ser feito depende de seu objetivo, o nosso aqui será explorar a porta aberta 21 protocolo FTP(File Transfer Protocol), responsável pelo serviço de envio de arquivos inteiros pela internet... para acessar esse serviço use:

```bash

ftp 192.168.0.10 21
```
Você irá se deparar com pedido de usuário e senha, está na hora da ferramenta Medusa, vamos usar a "força bruta".

### medusa

Aqui vai uma explicação sobre a ferramenta medusa:
Medusa é um quebra-senhas rápido e paralelo, responsável por ataques de força bruta, usando todas a variações possíveis das word-lists, criando muito ruído e sendo bastante escandaloso, mas muito usado ainda atualmente contra diversos serviços de rede, como SSH, FTP, HTTP, SMB e bancos de dados.

```bash
medusa -h 192.168.0.10 -U <word-list de usuários.txt> -P  <word-list de senhas.txt> -M ftp -t 2
#Os parâmetros -h é host, -U -P usuário e senha, -M define qual protocolo irá fazer o ataque e -t defini quantas tentativas vão ser feitas simultaneamente.
```
Deve-se procurar pela resposta [SUCCESS]

<img width="907" height="580" alt="0f5acb9a-856d-4ebe-b11e-05f0cfbbe8fe" src="https://github.com/user-attachments/assets/b325c519-d2be-4dce-9cfa-c8fcf4fe1b2e" />

Quando conseguir a senha e usuário, pode se logar, mas o objetivo já foi feito.

- ## SMB

O smb é um protocolo de compartilhamento de arquivos e recursos por LAN(local area network) normalmente alocado na porta 445, para sistemas Linux será usado o Samba(Conjunto de códigos abertos que permite sistemas Linux a utilizarem o protocolo smb).

para usar o serviço, precisaremos usar novamente o medusa para descobrir o usuário e a senha:

```bash
medusa -h 192.168.0.10 -U <word-list de usuários.txt> -P  <word-list de senhas.txt> -M smbnt -t 2
```

<img width="1026" height="495" alt="Captura de tela de 2026-11-12 21-10-28" src="https://github.com/user-attachments/assets/2b9aa017-ed6d-4eb2-bf15-e78084e23453" />

Após isso podemos fazer o que quisermos!

- ## DVWA

DVWA é um domínio feito no Metasploitable 2, um ambiente vulnerável e legalmente a ataques de força bruta, injeção de sql, xss(cross-site-scripting) e até erros básicos como senha o usuário no 


<img width="923" height="626" alt="Captura de tela de 2026-11-12 20-02-03" src="https://github.com/user-attachments/assets/cd0bf065-69d2-4c3b-8746-804d385217d3" />

Aqui devo passar um aviso importante, não podemos usar o medusa de forma convencional com (-M http), a web desse sistema possui um redirecionamento do protocolo http para "http-post-form" , mas o modulo (http) não lida bem com redirecionamentos HTTP (302) ele não segue o redirect automaticamente, e por padrão avalia sucesso/falha comparando o conteúdo da resposta com uma string que você define (ex: “FAIL” ou “Login failed”). Se o DVWA responde com 302 tanto no login certo quanto no errado (ou se a sessão/cookie muda a cada tentativa), o Medusa perde a referência e os resultados saem inconsistentes ou todos “falham”/”passam” igual.

- ## referencia

[medusa](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRgHpNc_URL2XE-6Jq1Z7Ya7ATo1KB819-fjBcnymgHpQ&s)
