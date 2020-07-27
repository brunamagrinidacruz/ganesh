# Aprofundando conceitos

Relembrando:
Vimos que pacotes são enviados entre dois hosts. Eles armazenam várias informações: mensagem, destinário, origem, número de pacotes e são definidos pelos protocolos.   
Na camada de rede, temos o protocolo IP que usa endereços e na camada de transporte pelos o modelo TCP que usa portas para identificação. Além disso, vimos o modelo de referência TCP/IP que tenta "teorizar" como funciona a comunicação entre as duas redes. Vimos também o modelo ISO/OSI que foi uma primeira tentativa mas é bem abstrato e longe da realidade, onde quem mais se aproxima (mas ainda nem tanto pois os conceitos se misturam na prática) é o modelo TCP/IP.

## Modelo de referência TCP/IP
```
+------------+
+ Aplicação  +
+ Transporte +
+ Internet   +
+ Host/Rede  +
+------------+
```

- Na camada de rede/host temos a rede em termos físico. 
- Na camada de internet temos de fato a camada de rede: endereçamento, roteamento de pacotes, como um pacote sai de uma máquina rede e chega em outra maquina na rede?
- Na camada de transporte temos o protocolo TCP controlando: dado que você encontrou a máquina, como você encontra o processo desejado. O TCP garante que a mensagem chegue e usa algumas checagens: Checksum que consiste em uma conta em cima do protocolo para descobrir se nenhum pacote se perdeu/foi alterado. Além disso ele é orientado a conexão e usa o three-way-handshake (três pacotes trocados para iniciar uma ocmunicação entre dois processos).

## Sockets

Socket é uma classe (ou uma struct em C). Essa estrutura contém host, porta, protocolo, estado (listening, blocked, etc) e outras informações. Em uma analogia, socket em inglês é "tomada", tanto a que vai na parede, quanto o plugue que se conecta a ela. O socket é isto, um socket que é a "tomada" (está em modo listening) e outro socket que é o "plug" (que vai se conectar a tomada)  
Uma conexão TCP é identificada por **um par** de sockets [host1, port1] e [host2, port2]. Mais de um host pode se conectar a um socket listening (na verdade, inumeros) e sua máquina pode ter vários sockets escutando diferentes sockets listenings.


## Portas

Uma porta é um número inteiro entre 0 e 65535. Mas o valor de 0 varia o significado em cada protocolo. No TCP a porta 0 é reservada e no UDP a porta 0 significa "no port".  
Além disso, cada protocolo tem seu próprio conjunto de portas. Então o protocolo TCP tem de 0 a 65535 portas e o UDP tem de 0 a 65535 portas. Se você tiver mais protocolos na máquina, terá mais portas para o protoloco especifico. Então é possível você ter algo conectado na porta 80/TCP e nada conectado na porta 80/UDP, ou ao contrário, ou os dois sim ou os dois não. Por isso, quando você vai se conectar a uma porta, é necessário indicar o protocolo também. Geralmente, quando não especificamos a porta, estamos falando do TCP.  
Ou seja, a porta não é nada mais que um número, um identificador. Quando você sobe um listening na sua máquina em uma porta, o SO fica sabendo que se alguém chegar com aquele número de porta, é você que procura. Apenas isso.  
Por convenção, temos as portas mais conhecidas (e usadas) que são de 0 até 1023 (<1024). Elas são padronizadas pois para acessar um certo processo em uma certa porta, ou você conhece a porta, ou você chuta todas, por isso, se convencionou que:
- 80: Servidor HTTP
- 443: Servidor HTTPS
- 22: SSH
- 20 e 21: FTP (o FTP tem duas portas pois uma é para envio de mensagens de controle e outro para envio de arquivos)
	
## Endereçamento IP: Máscaras de sub-rede

O endereço IP tem uma estrutura definida: ele informa qual o IP da rede e qual o IP do host.

Exemplo:
```
192	.168	.0	.1	/24
				[Mascara]
```
	
A mascara é o último valor que vai ao fim do IP e ela diz como vai funcionar a estrutura.  
No caso do exemplo, a mascara é 24. Então os 24 primeiros bits do endereço são o endereço da rede e os restantes são o endereço do host.  
Mas a MASCARA própriamente dita é a seguinte estrutura:
```
Mascara	->	      255	 .255	  .255	   .0
Em binario ->   11111111 11111111 11111111 00000000
```
Ou seja, a mascara diz como a estrutura vai funcionar. O correspondente da mascara em binario fala que os bits que tiverem em 1 na mascara, serão parte do endereço IP da rede, os que forem 0, serão parte do endereço IP do host.  
A mascara funciona com octantes. Então separamos 3 em 3 números decimais que foram 8 bits. 

```
Rede:		192	.168	.0	.0 /24 -> Este é o endereço da rede da Bruna
Rede:		192	.168	.1	.0 /24 -> Este é o endereço da rede da Loueny
```
```
Bruna: 		192	.168	.0	.1 /24
Loueny:		192	.168	.1	.1 /24
```
Neste exemplo acima, como um bit dentro os 24 que representam a rede no IP são diferentes, então se trata de redes DIFERENTES. Ou seja, como é uma mascará de 24 bits, se qualquer número mudar nos primeiros 3 octantes (nos primeiros 24 bits) se trata de uma rede diferente.
```
Bruna: 		192	.168	.0	.1 /24
Loueny:		192	.168	.1	.1 /24
Carol:		192	.168	.0	.2 /24	
```
Neste caso acima, como os 3 primeiros octantes são iguais para Bruna e Carol e estamos falando da mesma mascará de rede (24 bits), as duas máquinas estão na mesma rede. Entretanto, a Loueny não está (um dos 24 bits é diferente)*  
O endereço da Bruna na rede é 1 e o endereço da Carol na rede é 2.  
Mas claro, há outro tipo de mascaras, como a mascara de 16 bits.
```
Mascara -> 	255	 .255	  .0       .0
Em binario ->   11111111 11111111 00000000 00000000
```
Neste caso, o endereço da rede são os 16 primeiros bits, não os 24.

```
Rede: 		192	.168	.0	.0 /16

Bruna:		192	.168	.0	.1 / 16
Loueny:		192	.168	.1	.1 / 16
Carol:		192	.168	.0	.1 / 16
```

Agora, se trata de uma mascara de 16 bits. Os 16 primeiros bits são o endereço da rede (os dois primeiros octantes) enquanto o restante (os outros 16 bits) correspondem ao endereço da máquina. Por conta disto, Bruna, Carol e Loueny estão na mesma rede.

Mas por que ter varios tipos de mascaras? Não uma padrão? Para uma maior flexibilidade de configuração de rede. Então se precisamos que um número grande hosts se conectem a nossa rede, usamos uma mascara pequena para sobrar mais bits para representar o host. Por exemplo:
```
192	.168	.0	.0 / 8 
```
Aqui, serão 3 octetos só para endereços hosts.  

Em contrapartida, se queremos muitas redes com poucos hosts que possam se conectar, usamos uma mascara grande. Por exemplo:
```
192	.168	.0	.0 / 24
```
Neste exeplo acima, são 3 octetos só para representar redes, e apenas 8 bits para representar endereços.

Vale lembrar que a mascara não precisa ser multiplo de 8.
255	.255	.126	.0	(Mascara /17)

É possível fazer um monte de combinações com esse padrão, mas e quando as combinações acabam? Por que cada endereço deve ser único. Então... Já acabaram... Ai que surgiu o NAT.  
A ideia da NAT é você ter um número atrás de um número. Então imagina que no IPv4, antes de surgir a NAT, tratavamos endereços de IP's como casas, então cada casa tem seu número (sendo o mundo uma rua só haha). Agora com o NAT, temos que não são mais casas, são prédios. Então para achar alguém, vamos no prédio e do prédio vamos na casa. Desta forma, ganhamos novos bits para representação.  
Então, na prática do endereçamento IP, quando contratamos um serviço de internet, temos um IP público que todo o mundo pode acessar, e dentro desse IP público é feito outro mapeamento de IP's que são privados. Por isso, quando entramos em qualquer rede, o IP da nossa máquina pode ser igual, pois se trata do endereço PRIVADO da rede. Dentro da rede, aquele IP é único, mas a rede por si só tem um IP único em relação ao mundo.  
Desta forma, podemos ter 42 maquinas dentro de um único IP público. Ou seja, para quem está fora da casa, é como se o trafego efetuado nessas 42 maquinas fosse de uma única maquina, a que tem o IP público.

Para verificar o IP no Linux, podemos fazer
```
ip a
```
```
3: wlo1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 64:32:a8:96:0d:11 brd ff:ff:ff:ff:ff:ff
    inet 192.168.100.130/24 brd 192.168.100.255 scope global dynamic noprefixroute wlo1
       valid_lft 52425sec preferred_lft 52425sec
    inet6 fe80::7483:2476:d4d7:ef79/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
Podemos verificar aqui acima que 
```
192	.168	.100	.130 /24
```
Neste IP, usamos a mascara 24 e meu computador é a máquina 130 na rede. Mas esse IP é meu IP privado que está por trás de uma NAT (um IP público).

Agora, quando o Marlon tenta se conectar comigo, esse não é o IP que aparece pois esse IP não é o IP público. O IP que apareceé o IP privado, ou seja, o IP do meu roteador.
Caso queiramos fazer um servidor na nossa máquina, é necessário fazer um Port Forward.

## Interface de Rede

Uma interface de rede é como uma plaquinha de rede. Para ver as interfaces de redes, você pode usar esses comandos (usado no exemplo anterior). Todos significam a mesma coisa.
```
ip a
ip address
ip addr
```

Esse programa vai listar cada interface de rede da máquina.
A primeira interface é a interface de LOOPBACK.

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
```

Essa interface é local e todo mundo tem. Ela é virtual (não é uma placa de silicio no computador). Então quando dois programas na sua própria máquina (na mesma maquina) querem se conectar eles tem de usar um IP. Essa interface local define o IP local da sua máquina:
127	.0	.0	.1 / 8 Este é o equivalente ao localhost.

O restante das placas variam de computador a computador e dependem das maquinas físicas do host ou as máquinas virtuais.


