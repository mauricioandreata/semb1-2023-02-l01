# Questionário Sistemas Embarcados I

## 1. Explique brevemente o que é compilação cruzada (***cross-compiling***) e para que ela serve.

A compilação cruzada é uma maneira de ser produzir uma aplicação para uma plataforma diferente da qual o compilador está presente. No nosso caso, o compilador GCC por padrão gera arquivos executáveis compatíveis com os sistemas operacionais mais comuns, como Windows e Linux. No entanto nosso microcontrolador estudado STM32 não possui nenhum SO, assim é necessário o uso da compilação cruzada para gerar arquivos executáveis para o STM a partir do compilador GCC, assim podemos criar nosso código em uma IDE mais confortável para depois o enviar para o sistema embarcado.

## 2. O que é um código de inicialização ou ***startup*** e qual sua finalidade?

O código de inicialização (startup) é o responsável por inicializar nosso microcontrolador até que nosso programa executável possa ser executado, já que quando enviamos nosso executável para a memória do microcontrolador ele não é imediatamente executado. Assim, é necessário realizar uma série de tarefas até que o controle do nosso microcontrolador vá para a main() de nosso programa.

## 3. Sobre o utilitário **make** e o arquivo **Makefile responda**:

#### (a) Explique com suas palavras o que é e para que serve o **Makefile**.

O make é um utilitário usado para automatizar o processo de compilação do nosso código, as instruções para a compilação e os programas a serem compilados estão em um arquivo chamado Makefile. Com o make não é mais necessário realizar diversas vezes o comando do compilador GCC (no nosso caso arm-none-eabi-gcc -c -mcpu=cortex-m4 -mthumb -O8 -Wall) para todos os arquivos de um determinado projeto, assim otimizando o nosso tempo e facilitando caso algum arquivo tenha que ser atualizado e o projeto tenha que ser todo compilado novamente.

#### (b) Descreva brevemente o processo realizado pelo utilitário **make** para compilar um programa.

O utilitário make faz nada mais do que realizar comandos em nosso terminal a partir de instruções descritas no arquivo Makefile. O procedimento é do make é descrito a seguir, a partir de regras descritas no Makefile, são passados targets (arquivos alvos a serem criados) e seus prerequisites (dependências do arquivo alvo). O make verifica se o arquivo alvo de uma regra está atualizado, ou seja, se ele não existe ou se é mais antigo de que alguma dependência, caso esteja desatualizado o make criará um novo arquivo alvo utilizando a instrução descrita na regra e os arquivos dependência passados anteriormente, utilizando a seguinte sintaxe:

#### targets: prerequisites
####	 recipe

#### (c) Qual é a sintaxe utilizada para criar um novo **target**?

A sintaxe para se criar um novo target é
#### target: 
Onde target é o nome do arquivo a ser criado ou atualizado.

#### (d) Como são definidas as dependências de um **target**, para que elas são utilizadas?

As dependências de um target nada mais são que os arquivos necessários para se criar o target. Por exemplo, para se criar um arquivo objeto realocável .o de um programa é necessário compilar um código base .c. Assim, dizemos que esse código .c é uma dependência do arquivo .o.

#### (e) O que são as regras do **Makefile**, qual a diferença entre regras implícitas e explícitas?

Uma regra do Makefile é uma instrução para o utilitário make que o instrui a saber qual código realizar no terminal. Regras tem a sintaxe já descrita no item b. Regras explícitas deixam claro quais os arquivos específicos a serem atulizados (targets), quais suas dependências e qual a instrução dessa regra, como no exemplo abaixo:
#### main.o: main.c
#### 	arm-none-eabi-gcc -c -g -mcpu=cortex-m4 -mthumb -O0 -Wall main.c -o main.o
Já as regras implícitas tentam tornar as instruções da maneira mais genérica possível, através de variáveis automáticas como:
"$@" (representam o target da regra), 
"$<" (representa a primeira dependência) e 
"$^" (representa todas as dependências), 
ou %.o que lista todos os arquivos .o, 
além de ser possível criar novas variáveis com a sintaxe $(nome_da_variavel). Um exemplo de regra implícita é mostrado abaixo:

#### $(OBJDIR)/%.o:%.c
####	 $(CC) -c $(CFLAGS) $(DEPFLAGS) $< -o $@

## 4. Sobre a arquitetura **ARM Cortex-M** responda:

### (a) Explique o conjunto de instruções ***Thumb*** e suas principais vantagens na arquitetura ARM. Como o conjunto de instruções ***Thumb*** opera em conjunto com o conjunto de instruções ARM?

### (b) Explique as diferenças entre as arquiteturas ***ARM Load/Store*** e ***Register/Register***.

### (c) Os processadores **ARM Cortex-M** oferecem diversos recursos que podem ser explorados por sistemas baseados em **RTOS** (***Real Time Operating Systems***). Por exemplo, a separação da execução do código em níveis de acesso e diferentes modos de operação. Explique detalhadamente como funciona os níveis de acesso de execução de código e os modos de operação nos processadores **ARM Cortex-M**.

### (d) Explique como os processadores ARM tratam as exceções e as interrupções. Quais são os diferentes tipos de exceção e como elas são priorizadas? Descreva a estratégia de **group priority** e **sub-priority** presente nesse processo.

### (e) Qual a diferença entre os registradores **CPSR** (***Current Program Status Register***) e **SPSR** (***Saved Program Status Register***)?

### (f) Qual a finalidade do **LR** (***Link Register***)?

### (g) Qual o propósito do Program Status Register (PSR) nos processadores ARM?

### (h) O que é a tabela de vetores de interrupção?

### (i) Qual a finalidade do NVIC (**Nested Vectored Interrupt Controller**) nos microcontroladores ARM e como ele pode ser utilizado em aplicações de tempo real?

### (j) Em modo de execução normal, o Cortex-M pode fazer uma chamada de função usando a instrução **BL**, que muda o **PC** para o endereço de destino e salva o ponto de execução atual no registador **LR**. Ao final da função, é possível recuperar esse contexto usando uma instrução **BX LR**, por exemplo, que atualiza o **PC** para o ponto anterior. No entanto, quando acontece uma interrupção, o **LR** é preenchido com um valor completamente  diferente,  chamado  de  **EXC_RETURN**.  Explique  o  funcionamento  desse  mecanismo  e especifique como o **Cortex-M** consegue fazer o retorno da interrupção. 

### (k) Qual  a  diferença  no  salvamento  de  contexto,  durante  a  chegada  de  uma  interrupção,  entre  os processadores Cortex-M3 e Cortex M4F (com ponto flutuante)? Descreva em termos de tempo e também de uso da pilha. Explique também o que é ***lazy stack*** e como ele é configurado. 


## Referências

### Básicas

[1] [STM32F411xC Datasheet](https://www.st.com/resource/en/datasheet/stm32f411ce.pdf)

[2] [RM0383 Reference Manual](https://www.st.com/resource/en/reference_manual/rm0383-stm32f411xce-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)

[3] [Using the GNU Compiler Collection (GCC)](https://gcc.gnu.org/onlinedocs/gcc/index.html)

[4] [GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

### Vídeos Microsoft Teams

[5] [05 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[6] [06 - Arquitetura Cortex-M Parte 1/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[7] [07 - Arquitetura Cortex-M Parte 2/2](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[8] [08 - Microcontroladores STM32](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

[9] [10 - Introdução à arquitetura de computadores](https://web.microsoftstream.com/embed/channel/f6b3a0de-e6f3-4652-b2d5-f1164032498a?app=microsoftteams&sort=undefined&l=pt-br#)

### Material Suplementar

[5] [A General Overview of What Happens Before main()](https://embeddedartistry.com/blog/2019/04/08/a-general-overview-of-what-happens-before-main/)
 
[6] [Bare metal embedded lecture-1: Build process](https://youtu.be/qWqlkCLmZoE?si=mn5yDnJYudQ1PpZH)
 
[7] [Bare metal embedded lecture-2: Makefile and analyzing relocatable obj file](https://youtu.be/Bsq6P1B8JqI?si=yuNLPj3JQ-2IT1yo)
 
[8] [Bare metal embedded lecture-3: Writing MCU startup file from scratch](https://youtu.be/2Hm8eEHsgls?si=c27MpZ47ApiMSwHR)
 
[9] [Lecture 15: Booting Process](https://youtu.be/3brOzLJmeek?si=MsHRUEJP8zofjwJQ)