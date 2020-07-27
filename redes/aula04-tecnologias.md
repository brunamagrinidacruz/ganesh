#				Outras tecnologias

##			Domain Name System (DNS)

Os servidores DNS (Domain Name System, ou sistema de nomes de domínios) são os responsáveis por localizar e traduzir para números IP os endereços dos sites que digitamos nos navegadores.
Para isso existem os domínios e os servidores de DNS espalhados pelo mundo com a simples função de traduzir os endereços digitados, como o pet.icmc.usp.br, para o número de IP correspondente.

Ao invés de ter que colocar o IP no browser para acessar os sites, colocamos o Domain Name. Ou seja, invés de digitar 183.832.213.12 usamos pet.icmc.usp.br. E quem faz a conversão é o DNS.

O DNS é feito em uma estrutura de árvores debaixo para cima.

```
DNS-Root zone			
			         .
1º level zone		      /   |   \
			.com	.edu	.gov
2º level zone		/	/  \    / \
		google.com    
3º level zone	|	   \	
	drive.google.com   mail.google.com
```

O ponto é o dominio raiz de todo dominio. Ao digitar www.google.com estamos digitando www.google.com. (ponto no final).  
Na level 1º zone temos o responsável pela organização legal do domínio. Por exemplo, quem detinha o direito do .br era a FAPESP e para comprar um dominio .br era necessário falar com ela.

> More about: https://www.interserver.net/tips/kb/dns-dns-hierarchy/

Após entrar em contato do DNS, ele te devolve vários registros: A, AAAA, TXT, etc. Cada registro é um arquivo e te devolve uma coisa.
- A: Retorna o IPV4 do Google
- AAAA: Retorna o IPV6 do Google
- TXT: Conjunto de textos

Para analisarmos uma consulta DNS, podemos usar o dig.
```
dig pet.icmc.usp.br
```
```
; <<>> DiG 9.16.2 <<>> pet.icmc.usp.br
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 41771
;; flags: qr rd ra; QUERY: 1, ANhttps://ipok.com.br/blog/usando-dig-para-pesquisas-de-dns-parte-1/SWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;pet.icmc.usp.br.		IN	A

;; ANSWER SECTION:
pet.icmc.usp.br.	21599	IN	A	143.107.183.171

;; Query time: 16 msec
;; SERVER: 192.168.100.1#53(192.168.100.1)
;; WHEN: dom mai 03 00:11:54 -03 2020
;; MSG SIZE  rcvd: 60
```

O DIG (Domain Information Groper) é um utilitário que serve para realizarmos pesquisas de DNS. Ele funciona enviando consultas aos servidores e mostrando as respostas devolvidas por eles.   
Como no caso do exemplo, chamamos o dig para o pet.icmc.usp.br e recebemos um registro tipo A que devolve o IP do dominio.

Structure of DNS packet is:
```
+---------------------+
| Header              |
+---------------------+
| Question            | the question for the name server
+---------------------+
| Answer              | Answers to the question
+---------------------+
| Authority           | Not used in this project
+---------------------+
| Additional          | Not used in this project
+---------------------+
```

> Mais: https://ipok.com.br/blog/usando-dig-para-pesquisas-de-dns-parte-1/

## DHCP: Dynamic Host Control Protocol

É responsável por entregar IP's em uma determina rede.
Não necessáriamente a cada vez que nos conectamos em uma rede vamos receber o mesmo IP pois o DHCP trabalha com IP's dinamicos que são atribuidos. Ele possui um pool de IPs que podem ser um ou mais, por exemplo, um pool de IPs apenas para celulares.
Então, ao se conectar a uma rede, a maquina deve solicitar um IP para o DHCP.

##	Virtual Private Network - VPN

Nosso pacote às vezes trafegam sem criptografia, então é possível ver toda a informação dos pacotes interceptados. Isso acontece em utilização de wifi público e/ou aberto, empresa com varias filiais, trabalho remoto com necessidade de acessar a rede local da empresa e etc.
Uma maneira de resolver é o VPN que usa dois conceitos: tunelamento e criptografia.
No tunelamento, cria-se um túnel virtual entre 2 nós da rede. 
Agora com o túnel, os dados ainda estão expostos. Então criptografa os dados antes de entrar e descriptografar na saída.

## FTP: File Transfer Protocol

É o protocolo de transferência de arquivos e serve para transferir arquivos de um computador para o outro na rede.
Este protocolo usa duas portas (20 e 21 por padrão). Uma das portas serve apenas para transferências dos dados, mas para pedir o arquivo usamos a outra porta, a porta de controle (uma porta de comandos).
- 20: dados
- 21: controle

O protocolo tem dois modos de operação:
- Ativo: o cliente escolhe uma porta acima da 1024, como a 1026 por exemplo, e se conecta no servidor FTP na porta 21. Ou seja, o FTP tem um servidor listening na porta 21 e o cliente se conecta nela a partir da porta escolhida 1026. Quando a conexão é estabelecida, o servidor FTP adiciona +1 ao valor da porta e tenta se conectar nessa porta a partir da porta 20.  
Então o cliente se conecta da 1026 na 21, e o servidor FTP+1 a porta gerando a porta 1027 e tenta se conectar nela a partir da porta 20.
O problema desta abordagem é que o cliente deve ficar ouvindo na porta 1027.
- Passivo: nesta abordagem, o cliente faz as duas conexões e o servidor só espera a conexão.  
Então o cliente escolhe uma porta aleatória como a 1026 e conecta na 21 do servidor. Ao se conectar com o FTP, o FTP responde um número de porta que o cliente deve se conectar para enviar os dados. Suponha que ele retorne 2024. Depois disso, o cliente escolhe uma segunda porta como a 1027 e se conecta a porta 2024 do FTP.

> Mais: https://sempreupdate.com.br/comandos-ftp-linux-unix/
> Mais: https://itsfoss.com/set-ftp-server-linux/

##	Wi-fi handshake breaking

Descobrir a senha de uma rede Wi-Fi
	
1. Sniffar pacotes: capturar os pacotes que chegarem na interface de rede. 
2. Em algum momento, vai ter uma conexão legítima. E ao fazer essa conexão legitima, a pessoa faz um handshake (não o mesmo do TCP, mas com a mesma ideia). Esse pacote vem com uma senha na forma de uma hash.
3. Capturar o pacote de handshake.
4. Realizar um ataque de bruteforce usando um dicionário de senhas. Como? Temos um arquivo com várias senhas populares e pegamos cada uma das senhas e geramos um pacote e vemos se for ao pacote que capturamos. Se for igual, necessariamente a senha deve ser igual pois todos os outros parametros são conhecidos.  
Para realizar o passo 4, precimos do arquivo .cap de captura de pacotes, do aircrack-ng para fazer a decodificação da senha e um dicionario (como o rockyou.txt) para o aircrack-ng usar de comparativo. Vale lembrar que os pacotes das senhas são do tipo PROTOCOL: EAPOL

Exemplo:

```
termshark -r marcelobatata.cap
```
Ao fazer isso, podemos visualizar os pacotes. Para ver as keys podemos filtrar por  EAPOL

```
aircrack-ng marcelobatata.cap -w rockyou.txt
```
Ao fazer isso, vamos tentar verificar se a senha que existe em marcelobatata.cap para login da rede Wi-Fi existe no arquivo dicionario rockyou.txt e se existir, o aircrack-ng vai devolver a senha decofidificada

> Referências: https://www.youtube.com/watch?v=zAWcu3NQLME
