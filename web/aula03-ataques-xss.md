# Ataques

## Cross Site Scripting (XSS)
> https://owasp.org/www-community/attacks/xss/  

São ataques que podem ser realizados por meio de campos, parâmetros em URL. O objetivo é injetar um código malicioso dentro de uma página HTML.

Existem três tipos principais de XSS.

### Stored XSS Attacks
Neste ataque, o script malicioso está armazenado no servidor.   
Exemplo: um site que armazena mensagens dos clientes. A mensagem passada é um script que envia os dados de cookie (sessão) para um servidor alternativo. Essa mensagem fica salva no servidor (na lista de mensagens).

### Reflected XSS Attacks
O script malicioso está armazenado em alguma forma de conexão do cliente com o servidor, como a URL.  
Exemplo: colocar script em um campo ou colocar script malicioso na URL de forma que quando a pessoa acesse, as informações dela vá para o servidor alternativo.

**Ou seja, a diferença é o PAYLOAD. Onde o script malicioso está armazenado.**

### Dom Based XSS
?


## Testando os ataques
Para testar esses ataques, podemos utilizar a aplicação [`WebGoat`](https://owasp.org/www-project-webgoat/).

Para instalar com o Docker, siga os passos
```
# Baixando a imagem do WebGoat
docker pull webgoat/goatandwolf

# Subindo o container
docker run -p 8080:8080 --name WebGoat webgoat/goatandwolf
```
Para acessar, entre em http://localhost:8080/WebGoat/

## Prevenção
Para se prevenir contra esses ataques, https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html



Referências:
> https://www.welivesecurity.com/br/2017/07/07/vulnerabilidade-cross-site-scripting/  
> https://www.pmgacademy.com/tudo-sobre-xss/  
> https://littlemaninmyhead.wordpress.com/2018/06/24/demonstrating-reflected-versus-dom-based-xss/  