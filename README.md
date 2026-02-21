# Medusa-e-Kali-DIO2026
Este projeto foi desenvolvido como parte do desafio prático do curso, com o objetivo de:  Implementar ataques de força bruta em ambiente controlado  Compreender como ferramentas automatizam tentativas de autenticação  Documentar todo o processo técnico  Identificar vulnerabilidades e propor mitigação  Ambiente isolado, sem interações reais.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
* Ambiente Utilizado:
Kali Linux (máquina atacante)
Metasploitable 2 (máquina vulnerável)
Oracle VM VirtualBox
Medusa
## 📎 Evidências do Laboratório

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

medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6
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

ACCOUNT FOUND: [ftp] Host: 192.168.56.102 User: msfadmin Password: msfadmin

Isso demonstrou que:

Existe usuário válido

A senha é fraca

O serviço permite múltiplas tentativas sem bloqueio

✅ 6️⃣ Validação Manual do Acesso

Após identificar a credencial:

ftp 192.168.56.102

Resultado observado (conforme imagem):

230 Login successful.

Isso confirmou que a credencial descoberta realmente concede acesso ao sistema.

Aprendizado:
Ferramentas automatizadas devem sempre ter a validação manual para confirmar resultado.

🌐 7️⃣ Ataques em Formulários Web (DVWA)

A aplicação web vulnerável estava acessível via:

http://192.168.56.102/dvwa

Utilizando a aba Network (F12) do navegador, observei:

Método POST

Parâmetros enviados:

username

password

Login

Na área de Request, foi possível visualizar exatamente o que o servidor recebe.

Aprendizado importante:
Ferramentas como Medusa ou outras automatizam o envio dessas requisições, utilizando como critério de sucesso/erro a resposta retornada pelo servidor.

Se o sistema não implementa:

Rate limiting

CAPTCHA

Bloqueio de conta

Ele se torna vulnerável à automação.

🗂️ 8️⃣ Enumeração SMB e Password Spraying

Enumeração inicial:

enum4linux 192.168.56.102

Posteriormente, foi realizado teste com uma única senha para múltiplos usuários:

medusa -h 192.168.56.102 -U users.txt -p msfadmin -M smbnt

Esse método reduz chance de bloqueio e simula cenário corporativo mal configurado.
---------------------------------------------------------------------------------------------------
* Principais Aprendizados

A configuração correta da rede é fundamental.

Enumeração precede exploração.

Senhas fracas são o elo mais vulnerável.

Serviços sem limitação de tentativas são facilmente exploráveis.

A análise de requisições HTTP ajuda a entender como a automação funciona.

Validação manual é essencial após uso de ferramenta.
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
🛡️ 🔐 Recomendações de Mitigação
FTP

Implementar política de senhas fortes

Configurar bloqueio após múltiplas tentativas

Utilizar SFTP

Monitoramento de logs

Web

Rate limiting

CAPTCHA

Bloqueio de conta

Hash seguro (bcrypt/argon2)

SMB

Política de lockout

Desativar SMBv1

Monitoramento de eventos de autenticação
-----------------------------------------------------------------------------------------------
🎯 Conclusão

Este laboratório me permitiu compreender de forma prática:

Como ataques de força bruta funcionam

Como ferramentas automatizam tentativas

Como validar resultados

A importância de controles de autenticação

Foi desafiador inicialmente configurar a comunicação entre as VMs, mas após ajustes na rede Host-Only, consegui estruturar todo o ambiente e executar os testes com sucesso.














