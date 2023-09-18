# Assembly MIPS
## Padrão de escrita
Os códigos na linguagem de montagem assembly são escritos da seguinte forma:

```
.data

.text
```
No .data colocamos tudo relacionado aos registradores, e na parte .text colocamos nossas instruções.
## Registradores
No assembly não temos variáveis, e sim registradores. Ao todo temos 32 registradores, mas não podemos usar todos. Alguns exemplos de registradores comuns são:

** colocar imgs
  
## Instruções comuns
Algumas instruções comuns em assembly MIPS são:
- Load: movimentação de dados da memória pro registrador (coloca o valor de uma variável no registrador).
- Store: movimentação de dados do registrador para a memória.
- Move: movimentação de dados de um registrador para outro registrador.

## Comandos li $v0

** colocar imgs

## Tipos de dados

- .ascizz: cadeia de caracteres (Obs.: para trasnferir o conteudo da variavel pro registrador usa-se LI)
- .ascii: caractere (Obs.: para trasnferir o conteudo da variavel pro registrador usa-se LI)
- .word: inteiros (Obs.: para trasnferir o conteudo da variavel pro registrador usa-se LW, mas se o valor nao vier da memoria usa-se LI tmb)

