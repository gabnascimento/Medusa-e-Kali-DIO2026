# Medusa-e-Kali (Dio Lab 2026)
Este projeto foi desenvolvido como parte do desafio prático do curso, com o objetivo de:  Implementar ataques de força bruta em ambiente controlado  Compreender como ferramentas automatizam tentativas de autenticação  Documentar todo o processo técnico  Identificar vulnerabilidades e propor mitigação  Ambiente isolado, sem interações reais.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
* Ambiente Utilizado:
Kali Linux (máquina atacante) - https://www.kali.org/

Metasploitable 2 (máquina vulnerável) - https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

Oracle VM VirtualBox - https://www.virtualbox.org/

Medusa -Acessível no terminal do Kali Linux

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
🌐 2️⃣ Configuração de Rede

As duas VMs foram configuradas em modo Host-Only Adapter.

Essa foi uma das etapas mais desafiadoras para mim, pois inicialmente as máquinas não se comunicavam. Após revisar a configuração da rede Host-Only, consegui estabelecer conexão.
Apois isso inicializamos as duas maquinas virtuais, inicializando a do Metasploitable 2 jogando o comando "ip a" para assim identificar o IP da nossa máquina, na máquina Kali utilizamos o ping para dar um "oi" no IP da maquina a ser atacada e identificar se ela estava operante para que seguissemos com as demais atividades.

🔎 3️⃣ Enumeração Inicial
nmap -sV 192.168.56.102

Serviços identificados:

21/tcp → FTP (vsFTPd 2.3.4)

80/tcp → HTTP

445/tcp → SMB

Com base nisso, escolhi iniciar pelo FTP.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
📝 4️⃣ Criação das Wordlists

Foram criados dois arquivos simples:

users.txt
user
msfadmin
admin
root
pass.txt
123456
password
qwerty
msfadmin

Aprendizado importante:
Wordlists são combinações prováveis de usuários e senhas. Em ambientes reais, essas listas podem vir de vazamentos ou padrões previsíveis.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
⚔️ 5️⃣ Ataque de Força Bruta com Medusa (FTP)

Comando utilizado (conforme evidência da imagem):

medusa -h 000.000.00.102 -U users.txt -P pass.txt -M ftp -t 6
Explicação dos parâmetros:

-h → host alvo

-U → lista de usuários

-P → lista de senhas

-M ftp → módulo FTP

-t 6 → número de threads

Durante a execução, foi possível observar o Medusa testando cada combinação.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
📌 Resultado Observado

Na saída do terminal foi identificado:

ACCOUNT FOUND: [ftp] Host: (numero de ip da máquina) User: msfadmin Password: msfadmin

Isso demonstrou que:

Existe usuário válido

A senha é fraca

O serviço permite múltiplas tentativas sem bloqueio

-> Validação Manual do Acesso

Após identificar a credencial:

ftp 1xx.x6x.xx.101

Resultado observado foi o de login com sucesso.
Isso confirmou que a credencial descoberta realmente concede acesso ao sistema.

Aprendizado:Ferramentas automatizadas devem sempre ter a validação manual para confirmar resultado, além de que conteúdos são vazados diariamente, dentre eles senhas e usuaŕios que podem ser testados em vários ambientes e um deles pode ter o login edetuado com sucesso.

->  Ataques em Formulários Web (DVWA)

A aplicação web vulnerável estava acessível via:

http://xxx.1xx.xx.101/dvwa
(Visando seguir as boas práticas não colocarei o número de IP da minha máquina mesmo que seja um laboratório.)

Ferramentas como Medusa ou outras automatizam o envio dessas requisições, utilizando como critério de sucesso/erro a resposta retornada pelo servidor.

Se o sistema não implementa Rate limiting,CAPTCHA,Bloqueio de conta ele se torna vulnerável à automação.

---------------------------------------------------------------------------------------------------
* Principais Aprendizados

A configuração correta da rede é fundamental.

Enumeração precede exploração.

Senhas fracas são o elo mais vulnerável.

Serviços sem limitação de tentativas são facilmente exploráveis.

A análise de requisições HTTP ajuda a entender como a automação funciona.

Validação manual é essencial após uso de ferramenta.
Sistemas legados precisam ser revistos e atualizados para que não sejam submetidos a explorações.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
🔐 Dito isso, acredito que como recomendações de mitigação contra o FTP são: Implementar política de senhas fortes,Configurar bloqueio após múltiplas tentativas,Utilizar SFTP,Monitoramento de logs Web
,Rate limiting,CAPTCHA,Bloqueio de conta.
-----------------------------------------------------------------------------------------------

Conclusão

Este laboratório me permitiu compreender de forma prática:Como ataques de força bruta funcionam,a utilização e como rodam as ferramentas que automatizam tentativas de login, não obstabte, aprendi a como validar resultados e a importância de controles de autenticação.
Foi desafiador inicialmente configurar a comunicação entre as VMs, mas após ajustes na rede Host-Only, consegui estruturar todo o ambiente e executar os testes com sucesso.
Penso em outros diversos cenários que podem ser aplicados, mas acima de tudo, o curso fez com que aprendamos a nos defender e a defenser a instituição em que atuaremos.














