# Ataques 

## Dos: Denial of Service
São ataques de serviços não distribuidos (ou seja, apenas uma máquina, por isso é diferente de DDos). Vamos ver o SYN Flood e o Slow Loris.

### MITM: Man-In-The-Middle
É a interceptação de uma comunicação. Funciona com dois computadores (A e B) se comunicando. O atacante (M) finge ser B para o A e ser o A para o B.
```
	A ------------X------------ B
	
		\	    /
		      M
```

Quando o A enviar uma mensagem, ela vai chegar para o M que finge ser B. Depois, o M devolve para o B e o B acredita que o M é A. E assim por diante...
Mas como o M finge ser A e finge ser B? Com o ARP Spoofing é uma das maneiras.

Relembrando o Three-way handshake
1. Cliente SYN -> Servidor
2. Cliente <- SYN/ACK Servidor
3. Cliente ACK -> Servidor

#### SYN Flood (DoS)

Ele explora o Three-way handhskae: as vezes o segundo passo pode falhar. Ou seja, o servidor responde com uma SYN/ACK mas o Cliente não recebe ou algo parecido. Então o Cliente continua enviando a SYN (primeiro passo). O SYN Flood se aproveita desta falha para fazer um "flood" de SYN. Ou seja, no ataque SYN Flood, é enviado diversas solicitações SYN para o servidor e quando o servidor responde, o cliente SYN Flood ignora (como se não tivesse recebido e a conexão falhado) e continua fazendo solicitações para o servidor. Desta forma, o servidor acha que é uma conexão de uma máquina legitima que apenas não recebeu o SYN/ACK, o que não é verdade, pois o cliente SYN Flood recebeu mas apenas ignora.  
Ao fazer isto, o servidor fica sobrecarregado pois, pensando um servidor como uma casa com várias portas, todas elas ficam com a entrada ocupada e o servidor fica tentando abrir, mas quando abre, não há ninguém lá. Isso acontece pois o servidor tem uma tabela com um número máximo de conexões que podem ficar ativas, e o SYN Flood lota essa tabela com todas conexões suas pendentes, impedindo outro cliente a se conectar.

		Slow Loris (DoS)

Os pacotes funcionam assim: você tem um pacote HTTP que é um pacote grande e ele será quebrado em pacotes pequenos TCP (o TCP segmeneta o pacote HTTP). Ou seja, o pacote HTTP chega aos pedaços numa das partes (servidor e cliente). E alguma desses pacotes podem se perder ou se atrasar.   
Então, o que o Slow Loris faz? Ele atua como se fosse vários clientes e manda várias requisições simultaneamente. Entretanto, ele manda os pacotes de cada requisição com um grande intervalo de tempo entre cada ou incompletos.   
Uma outra forma desse ataque é o R U Deat Yet que funciona similar, só que para todos os clientes manda um mesmo pacote (com poucas alterações).
Exemplificação:
```
	Atacante    HTTPS Request Incompleto	Servidor
		--------------------------------
        Atacante    HTTPS Request Incompleto    Servidor
		------------------------------- 
	Atacante    HTTPS Request Incompleto 	Servidor
		-------------------------------
```
Ou seja, o host atacante manda uma requisição incompleta e o servidor fica esperando o tempo limite definido por ele para que o cliente complete a requisição. Quando esse tempo está perto de acabar, o cliente manda novamente, assim o servidor nunca fecha a conexão.

###	ARP Spoofing (Man-In-The Middle)

O ARP é o Address Resolution Protocol (Protocolo de Resolução de Endereços). O ARP ele descobre MAC Address dado que você tem o IP da máquina. Ele funciona em Broadcast na rede local, ou seja, todo mundo recebe tudo.

Relembrando:  
O MAC Address é o endereço físico de uma interface de redes. Ou seja, toda plaquinha de rede tem um endereço MAC (um endereço físico). Quando você se comunica com outra máquina localmente, você precisa do endereço MAC dela. 

O funcionamento do ARP é o seguinte:
```
	         Qual o MAC do IP 1 .2 .3 .4?
		/	   |		\
	Maquina 1	Maquina 2	Maquina 3
	1. 2. 3. 4      1 .3 .3 .7     7 .3 .3 .1
   AO:B1:C2:D3:11:00  FF:13:45:33...  E0:A1:02:E3:22...
```

Neste exemplo acima, o ARP está fazendo um ARP Request perguntando a todos.  
A Maquina1 vai receber e ver que é o IP 1 .2 .3 .4 e vai devolver um ARP Reply dizendo "O MAC do IP 1.2.3.4 é o A0:B1:C2:D3:11:00".  
Em seguida, o ARP Cache, que é uma tabela que guarda todos os IPs-MAC Address conhecidas, vai armazenar esse valor. Todo mundo tem essa tabela no computador e serve para não ter que fazer a consulta toda vez.

O Spoofing é falsificar o MAC Address dos outros.

Por exemplo, alguém pergunta qual é o MAC da Carol e a Carol responde (123), o ARP Spoofing é o ato de fingir ser o MAC da Carol e passar seu MAC no lugar. Ou seja, falar "Não, esse MAC tá errado, o MAC da Carol é (ABC)" - no caso, o MAC do atacante.

O Cache Poising é, após a Maquina 1 responder na exemplificação a seguir:
	
```
		 Qual o MAC do IP 1 .2 .3 .4?
	 Eu sou!/	   |		\
	       /           |             \
	Maquina 1	Maquina 2	Maquina 3
	1. 2. 3. 4      1 .3 .3 .7     7 .3 .3 .1
   AO:B1:C2:D3:11:00  FF:13:45:33...  E0:A1:02:E3:22...
```

O atacante M que está tentando se infiltrar responder que ele é o MAC Address desejeado. O protocolo ARP diz que nesses casos, ele deve só sobrescrever, mantendo a última ARP Reply.

```
		 Qual o MAC do IP 1 .2 .3 .4?
	        /	   |		\ O MAC do IP 1.2.3.4 é E0:A1:02:E3:22...
	       /           |             \
	Maquina 1	Maquina 2	Maquina 3
	1. 2. 3. 4      1 .3 .3 .7     7 .3 .3 .1
   AO:B1:C2:D3:11:00  FF:13:45:33...  E0:A1:02:E3:22...
```

Ou seja, no Cache Poising é o atacante M fingir ser A no ARP Cache do computador B e ser B no ARP Cache do computador A.

