# 🎀 Assembly MIPS
### Observação
Este documento está em construção e vai sendo complementado à medida que estudo.

## 🎀 Padrão de escrita
Os códigos na linguagem de montagem assembly são escritos da seguinte forma:

```
.data

.text
```
Usa-se o espaço ".data" para declaração de variáveis e constantes, e no espaço ".text" colocamos instruções.
## 🎀 Registradores
Em assembly MIPS, os registradores são unidades de armazenamento de dados e instruções usados pelo processador. Existem 32 registradores de uso geral, numerados de $0 a $31. Eles são usados para armazenar valores temporários, endereços de memória e parâmetros de função. Os registradores que podemos utilizar são:

** colocar imgs
  
## 🎀 Instruções comuns
Algumas instruções comuns em assembly MIPS são:
- Load: movimentação de dados da memória pro registrador (coloca o valor de uma variável no registrador).
- Store: movimentação de dados do registrador para a memória.
- Move: movimentação de dados de um registrador para outro registrador.

## 🎀 Comandos li $v0
| Registrador `$v0` | Função                                                         |
|--------------------|---------------------------------------------------------------|
| `$v0`, 1            | Imprime um inteiro.                                           |
| `$v0`, 2            | Lê uma inteiro.                                               |
| `$v0`, 3            | Lê um caractere.                                              |
| `$v0`, 4            | Imprime uma string terminada por null (ponteiro em `$a0`).    |
| `$v0`, 5            | Lê um inteiro (resultado em `$v0`).                            |
| `$v0`, 6            | Lê um double (resultado em `$f0` ou `$f0` e `$f1` em MIPS64).  |
| `$v0`, 10           | Encerra o programa.                                           |


## 🎀 Tipos de dados

- .ascizz: cadeia de caracteres (Obs.: para trasnferir o conteudo da variavel pro registrador usa-se LI)
- .ascii: caractere (Obs.: para trasnferir o conteudo da variavel pro registrador usa-se LI)
- .word: inteiros (Obs.: para trasnferir o conteudo da variavel pro registrador usa-se LW, mas se o valor nao vier da memoria usa-se LI tmb)

