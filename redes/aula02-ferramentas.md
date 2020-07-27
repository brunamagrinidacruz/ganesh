# Ferramentas

## Netcat
Netcat is a utility capable of establishing a TCP or UDP connection between two computers, meaning it can write and read thourgh an open port.
Netcat functions as a back-end tool that allows for port scanning and port listening. In addition, you can actually transfer files directly through Netcat or use it as a backdoor into other networked systems.
It was builted to be a tool of "back-end" that can be used by other programs and scrips. At the same time, it's a explorating redes tool.

O uso mais comum do netcet é criar uma conexão TCP com a porta especificada no host de destino tambem especificado. Sua entrada padrão (STDIN) é então enviada ao host e tudo o que retorna através da conexão é enviado à sua saída padrão (STDOUT). 

netcat [ip alvo] [porta]: conecta-se na [porta] do [ip alvo]
```
netcat 127.0.0.1 80
```

netcat -l -p [porta local]: cria um servidor que escuta possíveis conexões na [porta local] e, em seguida, faz a mesma leitura e escrita descrita acima
```
netcat -l -p 80
```

Algumas flags uteis:
```
-l: Coloca no netcat em estado de escuta
-p: Especifica a porta a ser usada
-u: Usar UDP ao invés de TCP
-n: Força o netcat a usar apenas endereços de IP numéricos, sem fazer consultas a servidores DNS (sem buscar a relação ip-site)
-e: Especifica o programa a ser executado depois de a conexão acontecer, ligando entradas e saídas ao programa
-s: Especifica o endereço IP da interface usada para enviar os pacotes. Pode ser usado para spoofing de IPS
-v: Torna verbose
-w: Limita o tempo máximo para que uma conexão seja estabelecida
```

Enviar arquivo do cliente para o ouvinte

```
nc -l -p [porta local] > [arquivo de saida]: ouve na porta porta local e armazena resultado no arquivo de saida.
```

```
nc -w3 [ip alvo] [porta] < [arquivo de entrada]: envia o arquivo de entrada para o ip alvo em uma porta.
```

Receber arquivo do ouvinte para o cliente

```
nc -l -p [porta local] > [arquivo de entrada]: ouve em uma porta local e prepara para enviar o arquivo de entrada.
```

```
nc -w3 [ip alvo] [porta] > [arquivo de saida]: conecta com o ip alvo em uma porta e envia o arquivo de saída.
```

Some fonts: 
> https://www.varonis.com/blog/netcat-commands/  
> https://www.mvps.net/docs/what-is-netcat-and-how-to-use-it/  
> https://gitbook.ganeshicmc.com/redes/ferramentas/netcat  

--------------------------------------------------------------------------------------------------------

## Nmap

Exploração de rede e rastreio de portas. Ele envia alguns dados (payload) para as portas e dependendo da resposta que ele receber do servidor, ele sabe qual o tipo de servidor (se é ssh, apache, banco de dados).
Ele também serve para:
- Encontrar IPs das máquinas da rede: Se você está em casa com várias pessoas mexendo no Wi-Fi, você pode descobrir o IP de cada um. Para fazer isso, o Nmap envia um pacote para todos os IP's e aquele que responder, ele sabe que está conectado.
- Encontrar portas abertas em um IP: Dado um IP da rede, descubra as portas abertas. Para fazer isso, ele envia um pacote para todas as portas e dependendo da respsota, ele sabe qual porta está aberta.
- Identificar sistemas operacionais
- Identifica qual programa está rodando em qual porta e qual versão também. Para descobrir qual versão, ele olha a resposta e compara com as respostas de cada versão.
Logo, com o Nmap, você consegue descobrir informações.

```
nmap -sC -sV [ip alvo]
```  
Sendo  
-sC: Para descobrir os programas que estão rodando  
-sV: Para descobrir as versões dos programas

Exemplo:
```
nmap -sC 127.0.0.1
```
```
Starting Nmap 7.80 ( https://nmap.org ) at 2020-05-01 17:03 -03
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00010s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info: 
|   Protocol: 10
|   Version: 5.7.29
|   Thread ID: 4
|   Capabilities flags: 65535
|   Some Capabilities: ODBCClient, SupportsCompression, IgnoreSpaceBeforeParenthesis, InteractiveClient, DontAllowDatabaseTableColumn, FoundRows, Support41Auth, LongColumnFlag, Speaks41ProtocolOld, SupportsLoadDataLocal, ConnectWithDatabase, IgnoreSigpipes, SwitchToSSLAfterHandshake, LongPassword, Speaks41ProtocolNew, SupportsTransactions, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: xNyV~]SWzqB\x05iFe\x1A\x16\x0B\x16	
|_  Auth Plugin Name: mysql_native_password
8080/tcp open  http-proxy
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Site doesn't have a title (application/json; charset=utf-8).

Nmap done: 1 IP address (1 host up) scanned in 5.97 seconds
```

Neste exemplo, fizemos um nmap na máquina local. Ele retornou duas portas abertas: 
- 3306 que está rodando um MySQL
- 8080 que está rodando um HTTP

----------------------------------------------------------------------------------------------------------------
				
## Netstat

Apresenta estatísticas sobre a rede de uma máquina que você está logado.  
Mostra as conexões sendo feitas, quais os programas que estão ouvindo em portas TCP, usar filtros e etc. Consegue descobrir se a conexão foi estabelecidade, quantidade de pacotes, informações sobre as tabelas de roteamento.

```
netstat -a
```
-a: Lista todas as conexões estabelecidas  
-at: Mostra todas as conexões mas só as TCPs  
-atp: Especifica o programa que está rodando nas portas  

Exemplo:
```
sudo netstat -atp
```
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 localhost:ipps          0.0.0.0:*               LISTEN      910/cupsd           
tcp        0      0 localhost:6463          0.0.0.0:*               LISTEN      2886/mainScreenPrel 
tcp        0      0 magrini.local:52310     whatsapp-cdn-shv-:https ESTABLISHED 13386/firefox       
tcp        0      0 magrini.local:60462     gru14s14-in-f14.1:https ESTABLISHED 13386/firefox       
tcp        0      0 magrini.local:36084     85.97.201.35.bc.g:https ESTABLISHED 13386/firefox       
tcp        0      0 magrini.local:52070     173.194.161.202:https   ESTABLISHED 13386/firefox       
tcp        0      0 magrini.local:38918     ec2-54-191-252-15:https ESTABLISHED 13386/firefox       
tcp        0      0 magrini.local:33498     162.159.135.234:https   ESTABLISHED 2843/Discord --type 
tcp6       0      0 [::]:8493               [::]:*                  LISTEN      16485/./frenchfries 
tcp6       0      0 localhost:ipps          [::]:*                  LISTEN      910/cupsd    

[Protocolo] [] [] [IP e porta] 			[]		   [Tipo de conexão] [Programa que está rodando]
```


----------------------------------------------------------------------------------------------------------------------

## NMAP vs NETSTAT

Os dois programas fazem Port Scan, mas o Netstat é localmente, ele dá mais detalhes mas é apenas no seu host.
Enquanto o NMAP é para fazer um Port Scan em qualquer máquina desde que você tenha acesso a ela, por exemplo algum servidor com IP Público, ou outra máquina conectada na mesma rede que você. Notar que o range padrão de porta dele é 0-1000.
