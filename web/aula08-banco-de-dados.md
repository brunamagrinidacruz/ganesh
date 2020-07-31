# SQL

## SQL Injection
É o ato de inserir scripts SQLS em um sistema para retornar dados sensíveis. Exemplo, suponha uma aplicação que faça o select com um nome recebido em um input
```
SELECT * FROM users WHERE name = "${nome}";
```
O usuário poderia passar como valor em nome:
```
' 1 = 1 --'
```
Isso irá gerar um script query
```
SELECT * FROM users WHERE name = '' 1 = 1 --';
```
Assim, ele iria recuperar todos os usuários do banco de dados.

> https://gitbook.ganeshicmc.com/web/semana-2/sql_injection

# Falhas de seguranças
Podemos ver as top 10 falhas de segurança em https://owasp.org/www-project-top-ten/