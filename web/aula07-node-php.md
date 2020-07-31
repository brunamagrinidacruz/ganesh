# NodeJS

## Typosquatting Attacks
É uma modalidade de ataque você se aproveita de erros de digitação do usuário, como URLS `nuubank.com`. No NodeJS, um estudante criou módulos com nome com erros de digitação de módulos comuns, como `expres` (sem o último `s`) e copiou o funcionamento do módulo original adicionando script (podendo ser malicioso ou não) que fazia funcionalidades da máquina. Isso é uma variação de Typosquatting Attacks e mais de 17k de pessoas baixaram a variação.

# PHP
O `.php` é um arquivo que mistura HTML com código PHP. Para diferenciar, usa-se a tag `<?php [codigo] ?>` e o código que está dentro da tag é um código PHP e deve ser interpretado.

## Command Injection
É quando conseguimos executar algo em um shell (terminal no sistema operacional). Isso é diferente de Coding Injection que é executar código PHP. Exemplo:
```
<?php
    print("Please specify the name of the file to delete");
    print("<p>");
    $file=$_GET['filename'];
    system("rm $file");
?>
```
Ao fazer a requisição `http://127.0.0.1/delete.php?filename=bob.txt;id"` ele irá interpretar como dois comandos diferentes: `rm bob.txt` e `id`. Isso pode ser usado de maneira maliciosa.
> https://gitbook.ganeshicmc.com/web/semana-2/command_injection

