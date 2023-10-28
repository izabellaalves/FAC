# Aula 1

### Operações lógicas

Nesta aula, iniciamos falando sobre as operações lógicas em Assembly MIPS, mais especificamente sobre as operações AND, OR, XOR e NOR.

Relembrando a tabela verdade de cada uma das operações:

AND: 

| A | B | A AND B |
|---|---|---------|
| 0 | 0 |   0     |
| 0 | 1 |   0     |
| 1 | 0 |   0     |
| 1 | 1 |   1     |

OR:

| A | B | A OR B |
|---|---|--------|
| 0 | 0 |   0    |
| 0 | 1 |   1    |
| 1 | 0 |   1    |
| 1 | 1 |   1    |

XOR:

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 |   0     |
| 0 | 1 |   1     |
| 1 | 0 |   1     |
| 1 | 1 |   0     |

NOR:

| A | B | A NOR B |
|---|---|----------|
| 0 | 0 |    1     |
| 0 | 1 |    0     |
| 1 | 0 |    0     |
| 1 | 1 |    0     |

### Operações lógicas em Assembly MIPS

Obs.: não é possível colocar o `$` antes do nome do registrador no Markdown, pois ele é interpretado como uma variável. Por isso, os `$` antes dos registradores foram ignorados.

- **AND t0, t1, t2**: t0 = t1 AND t2
- **OR t0, t1, t2**: t0 = t1 OR t2
- **XOR t0,t1, t2**: t0 = t1 XOR t2
- **NOR t0, t1, t2**: t0 = t1 NOR t2

### Máscaras

Estas operações lógicas são frequentemente usadas para criar máscaras, que são operações para lidar com bits. 

Por exemplo, a operação AND pode ser usada para extrair o valor de qualquer bit de um registrador. Para isso, basta fazer um AND entre o registrador e uma máscara que tenha apenas o bit desejado com valor 1. 

Já o XOR, pode ser usado para inverter o valor dos bits de um registrador, basta fazer um XOR entre o registrador e uma máscara que tenha todos os bits com valor 1.

Já para setar um bit, basta fazer um OR entre o registrador e uma máscara que tenha apenas o bit desejado com valor 1.

### Instruções Condicionais e Desvios

Em Assembly MIPS, temos 4 operações oficiais que podem ser utilizadas para realizar desvios condicionais, são elas:

- **slt**: set less than ou **slti**: set less than immediate
    - SLT t0, t1, t2: t0 = 1 se t1 < t2, t0 = 0 caso contrário.
- beq: branch equal
    - BEQ t0, t1, label: desvia para label se t0 == t1
- bne: branch not equal
    - BNE t0, t1, label: desvia para label se t0 != t1
- j: jump
    - J label: desvia para label

Obs.: O deslocamento relativo ao PC é a diferença entre o endereço da próxima instrução e o endereço da instrução de destino em um desvio. Para calculá-lo, conte o número de instruções entre o desvio e a instrução de destino, multiplique pelo tamanho de uma instrução (normalmente 4 bytes), e some ao endereço atual do PC. Isso permite encontrar o endereço absoluto da instrução de destino.

**End. de destino do desvio** = 4*label+PC

### Exemplo de desvio condicional
Em C:
```
if (numero1 >= numero 2) {
    numero1 = numero1 + numero2;
}
```

Assembly: 
```
.text
    main:
        $v0, 5
        syscall

        move $t1, $v0

        $v0, 5
        syscall

        move $t2, $v0

        slt $t0, $t1, $t2
        beq $t0, $zero, if
        j fim

        if:
            add $t1, $t1, $t1
        fim:
            li $v0, 10
            syscall
```
# Aula 2

### Instruções de desvio condicional

A aula 2 foi iniciada com um exemplo sobre instruções de desvio condicional.

Exemplo:

**Exemplo:**

Em C:
```
if (i==j) f=g+h;
else f = g-h;
```

Suponha que:
f - s0
g - s1
h - s2
i - s3
j - s4

Assembly:
```
.text
main:
	beq $s3, $s4, if
	sub $s0, $s1, $s2
	j fim
	
if:
	add $s0, $s1, $s2
		
fim:
	li $v0, 10
	syscall
```

### Laços

Podemos usar as instruções que já conhecemos de desvio condicional para implementar laços. Vamos ver um exemplo:

**Exemplo:**

Em C:
```
i = 0;
while (i<m) {
    A[i] = 0;
    i++;
}
```

Suponha que:

i = t0
m = s0
A = s1

Assembly:
```
add $t0, $zero, $zero

loop:
	slt $t1, $t1, $s0
	beq $t1, $zero, fim
	sll $t3, $t0, 2
	add $t3, $t3, $s1
	sw $zero, 0($t3)
	addi $t0, $t0, 1
	j loop

fim: 
	li $v0, 10
	syscall
```

### Procedimentos

Procedimentos são blocos de código que podem ser chamados de qualquer parte do programa. Eles são muito úteis para evitar repetição de código.

Durante a aula, o professor passou um fluxo que pode ser seguido para chamar um procedimento, ele está a seguir:

1) Coloque os argumentos nos registradores
2) Desvie a execução p/ o procedimento
3) Ajuste o armazenamento no procedimento
4) Execute o procedimento
5) Armazene o resultado nos registradores
6) Ajuste ao armazenamento
7) Retorne ao procedimento chamador

Os registradores utilizados nos procedimentos são:

- **a0-a3**: argumentos
- **v0-v1**: resultado
- **ra**: endereço de retorno
- **s0-s7**: armazenamento

### Pilha da memória

A pilha é uma região de memória usada para armazenar dados temporários e endereços de retorno durante a execução de programas. 
O "stack pointer" (SP) é um registrador utilizado em processadores e arquiteturas de computadores para gerenciar a pilha de memória. Ele aponta para o topo da pilha, ou seja, para o último endereço da pilha.

# Aula 3

### Pilha

O Stack Pointer cresce para baixo, ou seja, quando um dado é inserido na pilha, o SP é decrementado. Quando um dado é removido da pilha, o SP é incrementado.

Portanto, para inserir um dado na pilha, primeiro fazemos um addi no stack pointer, removendo a quantidade necessária para armazenar o dado. Depois, armazenamos o dado no endereço apontado pelo SP.

**Exemplo: Salvar s0 e s1 na pilha**
    
```
addi $sp, $sp, -8
sw $s0, 0($sp)
sw $s1, 4($sp)
```

Para remover um dado da pilha, primeiro carregamos o dado do endereço apontado pelo SP. Depois, fazemos um ADD no SP, adicionando a quantidade necessária para remover o dado da pilha.

**Exemplo: Remover s0 e s1 da pilha**
    
```
lw $s0, 0($sp)
lw $s1, 4($sp)
addi $sp, $sp, 8
```

### Chamada de procedimentos

Antes de fazermos um exemplo, vamos ver algumas instruções usadas na chamada e retorno ao procedimento, são elas:

- **jal**: jump and link
    - Jal procedimento: desvia para o procedimento e salva o endereço de retorno em $ra
- **jr**: jump register
    - Jr ra: desvia para o endereço de retorno salvo em ra.

**Exemplo:**

Em C:
```
int main() {
	int a = 3; // $s0
	int b = 2; // $s1
	
	printf("%d\n" media(a,b));
	return 0; 
}

int media(int a, int b) {
	return (a+b)/2; 
	// a = $a0
	// b = $a1
}
```

Assembly:
```
text
main:
	li, $a0, 3
	li $a1, 2
	
	jal media
	
	move $a0, $v0
	li $v0, 1
	syscall
	
	li $v0, 10
	syscall
	
media:
	add $s0, $a0, $a1
	srl $v0, $s0, 1
	jr $ra
```

Agora, faremos um novo exemplo, para praticar a manipulação da pilha.

**Exemplo:**

Em C:
```
int main() {
	int a = 3; // $s0
	int b = 2; // $s1
	
	printf("%d\n" media(a,b));
	return 0; 
}

int media(int a, int b) {
	return soma(a,b)/2; 
	// a = $a0
	// b = $a1
}

int soma(int a, int b){
	return a+b
}
```

Assembly:

```
.text
main:
	li $s0, 3
	li $s1, 2

	move $a0, $s0
	move $a1, $s1

	jal media

	move $a0, $v0
	$v0, 1
	syscall

	$v0, 10
	syscall

media:
	addi $sp, $sp, -4
	sw $ra, 0(sp)
	
	jar soma
	
	srl $v0, $v0, 1
	
	lw $ra, 0(sp)
	addi $s, $sp, 4
	
	jr $ra
	
soma:
	add $v0, $a0, $a1
	jr $ra
```

### Formato de instruções tipo J

As instruções tipo J são usadas para desvios incondicionais. Elas possuem 26 bits para o endereço de destino, que é o endereço da instrução para onde o desvio será feito. 
As instruções jal, jr e j são do tipo J.

|   Op    |   Endereço   |
|:--------:|:------------:|
|  6 | 26 |

### Manipulação de Caracteres

Na tabela ASCII, as letras maiúsculas têm valores decimais na faixa de 65 a 90, enquanto as letras minúsculas têm valores na faixa de 97 a 122. A diferença entre as letras maiúsculas e minúsculas é de 32.

'A' (maiúscula) tem um valor decimal de 65.
'a' (minúscula) tem um valor decimal de 97.


Para converter uma letra maiúscula em minúscula (ou vice-versa) sem usar linguagem de programação, você pode adicionar 32 ao valor decimal do caractere.

Para converter uma letra maiúscula em minúscula (ou vice-versa) usando máscara bit a bit, você pode usar a operação XOR bit a bit. A ideia é criar uma máscara que tenha o sexto bit definido como 1 (32 em decimal) e aplicar essa máscara ao caractere. 

```
'A' (65 em binário: 01000001)
XOR Máscara (32 em binário: 00100000)
Resultado ('a' em binário: 01100001)
```

### Instruções de acesso a memória para 1 byte

As instruções lb e sb são usadas em linguagem assembly MIPS para carregar e armazenar bytes em memória, respectivamente.

lb t0, 0(s0)   -> carrega 1 byte da memória em um registrador

sb t0, 0(s0)   -> armazena 1 byte na memória

### System Calls para caracteres

> 11 - imprime caracter
> 12 - lê caracter

# Aula 4

Na aula 4, iniciamos o conteúdo de Aritmética Computacional, que é a área da computação que estuda a implementação de operações aritméticas em computadores digitais.

As operações que vamos ver até a prova 2 são: adição, subtração e multiplicação.

#### Representação de números inteiros
Números inteiros podem ser representados com sinal (com intervalo de -2³¹ a 2³¹-1) ou sem sinal (com intervalo de 0 a 2³²-1).
- Overflow: número muito grande que a máquina não consegue representar.

#### Representaçõ de números reais
Números reais são representados em ponto flutuante, que é uma representação aproximada.
- Underflow: número muito pequeno que a máquina não consegue representar.
- Épsilon de máquina: menor número que a máquina consegue representar.

### Adição

A adição é feita somando bit a bit, e usando carries que são passados pro próximo bit a esquerda.

Observando a adição, é possível determinar em quais casos podemos ter overflow:

- Quando os dois números são positivos e o resultado é negativo.
- Quando os dois números são negativos e o resultado é positivo.

Ou seja, o overflow na **adição** ocorre quando os operandos possuem o mesmo sinal e o resultado possui sinal diferente

### Subtração

A subtração é feita subtraindo bit a bit, e usando borrows que são passados pro próximo bit a esquerda.

Observando a subtração, é possível determinar em quais casos podemos ter overflow:

- Quando o primeiro número é positivo e o segundo é negativo, e o resultado é negativo.
- Quando o primeiro número é negativo e o segundo é positivo, e o resultado é positivo.

Ou seja, o overflow na **subtração** ocorre quando os operandos possuem sinais diferentes.

### Implementação para números com sinal

Para verificar se houve overflow numa soma, podemos usar o seguinte código:

```
addu $t0, $t1, $t2
xor $t3, $t1, $t2
slt $t3, $t3, $zero
bne $t3, $zero, sem_overflow

xor $t3, $t1, $t0
slt $t3, $t3, $zero
bne $t3, $zero, overflow
j fim

sem_overflow:
    # código para quando não houver overflow
    j fim
```

Vamos simular um overflow e executar o código acima:

Se temos overflow, o sinal dos operandos será igual, então o nosso primeiro XOR terá valor 0. Ou seja, o registrador t3 terá valor 0.

Depois, fazemos SLT para verificar se t3 é menor que 0, no caso, t3 é igual a 0, portanto o SLT terá valor 0.

Depois, fazemos BNE para verificar se o valor de t3 é diferente de 0, isso é falso, portanto seguimos e não entramos no label "sem_overflow".

Se temos overflow, o sinal dos operadores será diferente do sinal do resultado, então faremos um segundo XOR, para comparar o valor de t1 com o valor de t0. Como eles possuem sinais diferentes, pois estamos simulando um overflow, o registrador t3 terá valor 1.

Depois, fazemos um SLT para verificar se o valor de $t3 é menor que 0, como t3 tem valor 1, isso é falso, portanto o SLT terá valor 0.

Depois, fazemos um BNE para verificar se o valor de t3 é igual a 0, isso é verdadeiro, portanto entramos no label "overflow".

### Implementação para números sem sinal

Temos agora como objetivo detectar um possível overflow após a adição dos valores contidos em t1 e t2, porém, agora estamos trabalhando com números sem sinal. 

Neste caso, temos overflow quando t1 + t2 > 2³²-1.

Ou seja, o valor da soma de t1 e t2 é maior do que o maior valor que a máquina consegue representar.

Para detectar se ocorre um overflow, a expressão t1 + t2 > 2³²-1 é reorganizada para t1 > 2³²-1 - t2. Isso significa que, se t1 for maior que a diferença entre o valor máximo sem sinal e t2, então ocorreu um overflow.

Em assembly, podemos verificar da seguinte forma:

```
addu $t0, $t1, $t2
nor $t3, $t2, $zero  # $t3 = 2³²-t2-1
slt #t3, #t3, $t1
bne $t3, $zero, overflow
```
# Aula 5

### Multiplicação

A multiplicação é feita multiplicando bit a bit, e usando shifts que são passados pro próximo bit a esquerda.

O primeiro operando da multiplicação é chamado de multiplicando, e o segundo é chamado de multiplicador. Já o resultado é chamado de produto.


1) Produto = 0
2) Para cada bit do multiplicador:
	1) Se o bit for 1, some o multiplicando ao produto, P = P + M
	2) Se o bit for 0, desloque o multiplicando 1 bit para a esquerda
4) Faça o deslocamento a direita em 1 bit no multiplicador
5) Se não for a n-ésima iteração, volte para o passo 2

n = número de bits do multiplicador.

### Implementação da multiplicação

Vamos multiplicar 2X3. Sendo 2 o multiplicando e 3 o multiplicador.

![Multiplicação 2x3](https://github.com/izabellaalves/FAC/blob/main/imagens/mult2x3.png?raw=true)








