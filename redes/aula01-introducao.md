# Definição de Rede de Computadores
Rede de computadores é uma estrutura de dispositivos conectados com o proposito de compartilhamento de informação e comunicação. São exemplos: internet, rede privada/militar/industriar, bluetooh e rádio.

## Funcionamento de uma rede
Quando tentamos entrar em um site, como https://icmc.usp.br, o nosso navegar está fazendo uma requisição para o ICMC. Mas há alguns passos antes disso acontecer.

###	DNS Server
Então, quando você tenta acessar um site, primeiro o navegador faz uma consulta no servidor DNS (DNS Server). O DNS server é como uma lista telefônica, onde há um link associado a um endereço IP. Ou seja, ao digitar https://icmc.usp.br, o browser verifica qual o endereço IP do site no DNS Server, ou seja, http://143.107.231.6.

###	Requisição
Após isto, é necessário fazer uma requisição. A requisição é como um documento que especifica as informações que você busca no servidor. Essa requisição contém pacotes (conjunto de informações) que leva a informação que você deseja enviar para o servidor. Vale lembrar que nem todas as redes de computadores usam sistemas de pacotes, como as máquinas de banco mas como é a maioria, vamos focar nisso.  
Mas para esse pacote ser entendido, é necessário definir um protocolo (conjunto de normas). O protocolo define como o pacote está organizado, como deve ser interpretado, o que fazer com ele, etc...

Exemplo: no caso de um envio de mensagem minha para a Loueny
```
|BRUNA|LOUENY|VOCÊ É LINDA|
 ORIGEM DESTINO MENSAGEM
```
O pacote é o contexto "|BRUNA|LOUENY|VOCÊ É LINDA|" e o protocolo define que em primeiro no pacote estará a origem, depois o destino e por último a mensagem. Ao enviar um pacote, ele chega como 0 e 1 no servidor, por isso é necessário um protocolo, ele é um manual para entender como ler a mensagem, qual o tamanho de cada campo e etc. 

## Protocolo
O protocolo veio para solucinar as questões: "para quem é o pacote?" "qual o tamanho esperado do pacote?" "qual o formato da informação?" "há mais de um pacote para essa comunicação?" "o pacote é valido?". Mas claro, o pacote é bem mais que um conjunto de regruinhas, ele diz como ler os pacotes, como rearranjar os pacotes caso cheguem fora de ordem e etc.

```
Tamanho| Intervalo | Descricao	
4 bits 	   0 - 3      Tamanho do pacote
4 bits	   4 - 7      Destinário
28 bits	   8 - 35     Mensagem
4 bits	   36 - 39    Numero do pacote (caso haja mais de um pacote é útil para saber qual a ordem do pacote)
4 bits     40 - 43    Validade do pacote
4 bits     44 - 47    Fim do pacote (caso haja mais de um pacote, sabemos que esse pacote é o último ou não)
```
```
-----------------
|0|1|0|1|0|1|1|0|
|0|1|1|0|0|1|0|1|
|0|1|1|0|1|0|0|1|
|0|1|1|1|1|0|1|0|
|0|0|0|0|1|0|1|0|
-----------------
```

###	Protocolo IP
O protocolo IP é o protocolo da internet. Todo dispositivo conectado a internet usa este protocolo. Basicamente, o protocolo IP define uma forma de achar hosts (máquinas/placas de redes). Ou seja, dado o IP da Carol, encontre e entregue uma mensagem pra ele.  
Este protocolo usa pacotes e permite que você enderece um pacote e envie à internet (como enviar uma mensagem). Além disso, esse protocolo não exige conexão física direta. Podemos nos comunicar com alguém no Canadá.
O protocolo IP usa um número IP que é basicamente uma sequência de números que identificam um host (uma máquina).

### Protocolo TCP (Transmission Control Protocol)
Diferente do protocolo de IP, o protocolo TCP trabalha na camada de *transporte*. Ou seja, a função dele é um pouco diferente do protoloco IP. O protocolo TCP cria uma conexão (ativa ou inativa) entre dois processos em dois hosts. Além disso há possibilidade de enviar mensagens biredicionalmente, isto ganha o nome de FullDuplex.  
Observação: 
- Duplex: apenas um host pode mandar mensagem
- HalfDuplex: pode ir e pode voltar, mas não ao mesmo tempo
- FullDuplex: há mensagens indo e voltando simultaneamente  

O TCP usa portas que são números que identificam os processos que rodam no computador.  
O Checksum, que é uma parte do pacote que guardamos a quantidade de bits no envio, e na chegada, verificamos se é igual. (https://br.ccm.net/faq/1174-como-verificar-a-integridade-do-download-com-o-md5sum).  
E por último, o Protocolo TCP usa o three-way handshake, que é o começo da comunicação. Então no método three-way handshake, o host faz uma solicitação de comunicação (como um computador entrar em um site), o site responde perguntando se o host quer mesmo se comunicar e na terceira comunicação, o host responde "sim" e é iniciada.  
Usando uma analogia, o Protocolo TCP é como se fosse um tubo que transporta água. Cada host é uma estação e essa estação tem vários lugares que o tubo pode ser encaixado. Ao tentar entrar em um site, o seu computador é uma estação e o servidor é outra, e a porta utilizada é onde deve ser encaixado o tubo, por isso há uma porta de entrada e uma de saída (uma no computador e uma no servidor)

### Protocolo UDP
O protocolo UDP (User Datagram Protocol) atua na mesma camada que o protocolo TCP *camada de transporte*. Então em uma comunicação, você usa o protocolo UDP ou o TCP (você pode usar outros também). Ele tenta atingir o mesmo objetivo que o TCP mas de forma diferente. 
Ele é mais leve que o protocolo TCP mas porque: ele não estabelece conexão, não garante a integridade da mensagem, não garante que a mensagem irá chegar e etc. Mas por que usar ele? Ele é usado em aplicações tolerantes a corrupção/perda parcial das mensagems, como em Streaming (Youtube, Hangouts) onde é mais importante não parar a experiência do que perder a qualidade (pacotes), DNS e etc.

##		Modelo ISO/OSI
Na explicação dos protocolos, falamos sobre camadas (rede e transporte). Essas camadas são desse modelo.
Esse modelo é um modelo teórico da estruturação da rede. 

```
+-----------------+
+ 1: Aplicação    +
+ 2: Apresentação +
+ 3: Sessão       +
+ 4: Transporte   +
+ 5: Rede         +
+ 6: Enlace       +
+ 7: Física       +
+-----------------+
```

A ideia é que na hora de consultar algo no browser, ele passa das camadas 1 ao 7, até chegar na física e depois volta de 7 a 1 até chegar na camada aplicação do servidor solicitado.
Mas na prática, o modelo é bem diferente.	

##		Modelo TCP/IP
Esse modelo também é teórico que tenta organizar a informação aos moldes do modelo TCP/IP. Então o Modelo ISO/OSI (que existia antes do TCP e IP), não se adequa muito ao TCP/IP. O Modelo TCP/IP tenta explicar melhor o que atualmente acontece na prática.

```
+------------+
+ Aplicação  +
+ Transporte +
+ Internet   +
+ Host/Rede  +
+------------+
```

**Resumindo, o modelo OSI é referenciar para "o que fazer?" e o modelo TCP/IP é referência para "como fazer?"**

