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
Já as regras implícitas tentam tornar as instruções da maneira mais genérica possível, através de variáveis automáticas como 
"$@" (representam o target da regra), 
"$<" (representa a primeira dependência) e 
"$^" (representa todas as dependências), 
ou %.o que lista todos os arquivos .o, 
além de ser possível criar novas variáveis com a sintaxe $(nome_da_variavel). Um exemplo de regra implícita é mostrado abaixo:

#### $(OBJDIR)/%.o:%.c

## 4. Sobre a arquitetura **ARM Cortex-M** responda:

### (a) Explique o conjunto de instruções ***Thumb*** e suas principais vantagens na arquitetura ARM. Como o conjunto de instruções ***Thumb*** opera em conjunto com o conjunto de instruções ARM?

O conjunto de instruções Thumb é uma extensão do conjunto ARM, projetada para ser mais compacta (instruções de 16 bits) e economizar espaço de código. Suas principais vantagens incluem códigos menores, melhor eficiência de cache e menor consumo de energia. Thumb opera em conjunto com o conjunto ARM, permitindo que o processador alterne dinamicamente entre os modos Thumb e ARM, oferecendo flexibilidade de otimização entre tamanho de código e desempenho.

### (b) Explique as diferenças entre as arquiteturas ***ARM Load/Store*** e ***Register/Register***.

A arquitetura ARM Load/Store utiliza instruções específicas para transferir dados entre registradores e memória, enquanto a Register/Register executa operações diretamente entre registradores, eliminando a necessidade de instruções de carga/armazenamento. Register/Register é geralmente mais eficiente para operações puramente em registradores, enquanto Load/Store oferece mais flexibilidade em relação à manipulação de dados na memória.

### (c) Os processadores **ARM Cortex-M** oferecem diversos recursos que podem ser explorados por sistemas baseados em **RTOS** (***Real Time Operating Systems***). Por exemplo, a separação da execução do código em níveis de acesso e diferentes modos de operação. Explique detalhadamente como funciona os níveis de acesso de execução de código e os modos de operação nos processadores **ARM Cortex-M**.

Nos processadores ARM Cortex-M, os níveis de acesso (Privileged e Unprivileged) e os modos de operação (Thread e Handler) são fundamentais para sistemas com RTOS. Os níveis de acesso diferenciam entre código privilegiado (kernel) e não privilegiado (aplicações), enquanto os modos de operação separam o código em modo Thread (execução normal) e modo Handler (tratamento de exceções). A combinação desses conceitos permite a execução segura e eficiente de RTOS, isolando as tarefas do sistema operacional e garantindo a resposta a interrupções e exceções em tempo real.

### (d) Explique como os processadores ARM tratam as exceções e as interrupções. Quais são os diferentes tipos de exceção e como elas são priorizadas? Descreva a estratégia de **group priority** e **sub-priority** presente nesse processo.

Os processadores ARM tratam exceções e interrupções por meio de um sistema prioritário. Existem dois tipos principais de exceções: exceções síncronas (como instrução não válida) e exceções assíncronas (como interrupções). Elas são priorizadas com base em sua natureza. A estratégia de group priority e sub-priority permite subdividir prioridades em grupos e ajustar prioridades dentro de um grupo, proporcionando uma gestão mais detalhada da priorização de exceções e interrupções.

### (e) Qual a diferença entre os registradores **CPSR** (***Current Program Status Register***) e **SPSR** (***Saved Program Status Register***)?

O CPSR (Current Program Status Register) armazena o estado atual do processador, enquanto o SPSR (Saved Program Status Register) armazena temporariamente o estado anterior ao ser usado em situações de troca de contexto, como em chamadas de interrupção ou exceções, para possibilitar o retorno ao estado anterior após o tratamento dessas situações.

### (f) Qual a finalidade do **LR** (***Link Register***)?

O Link Register é o registrador responsável por armazenar o endereço de retorno após uma interrupção ou chamada de outro processo. Ele é importante para a organização do controle de fluxo em programas, especialmente em chamadas de funções.

### (g) Qual o propósito do Program Status Register (PSR) nos processadores ARM?

O Program Status Register (PSR) é o registrador que armazena as informações sobre o estado atual do processo. Ele armazena dados como flags de condição, status de interrupções, contexto do programa.

### (h) O que é a tabela de vetores de interrupção?

A tabela de vetores de interrupção é uma estrutura de dados que armazena e mapeia os endereços de memória correspondentes às instruções de tratamento de interrupção do nosso processador.

### (i) Qual a finalidade do NVIC (**Nested Vectored Interrupt Controller**) nos microcontroladores ARM e como ele pode ser utilizado em aplicações de tempo real?

O NVIC ARM gerencia interrupções, permitindo prioridades e tratamento aninhado de interrupções. Ele é crucial em aplicações de tempo real, pois oferece um controle eficiente e flexível sobre o tratamento de eventos assíncronos, garantindo que as interrupções sejam processadas de acordo com suas prioridades e permitindo a execução de múltiplas interrupções em um ambiente concorrente.

### (j) Em modo de execução normal, o Cortex-M pode fazer uma chamada de função usando a instrução **BL**, que muda o **PC** para o endereço de destino e salva o ponto de execução atual no registador **LR**. Ao final da função, é possível recuperar esse contexto usando uma instrução **BX LR**, por exemplo, que atualiza o **PC** para o ponto anterior. No entanto, quando acontece uma interrupção, o **LR** é preenchido com um valor completamente  diferente,  chamado  de  **EXC_RETURN**.  Explique  o  funcionamento  desse  mecanismo  e especifique como o **Cortex-M** consegue fazer o retorno da interrupção. 

Quando uma interrupção ocorre no Cortex-M, o valor do LR (Link Register) é automaticamente preenchido com um valor especial chamado EXC_RETURN. Esse valor indica ao processador como restaurar o contexto interrompido. O EXC_RETURN contém informações sobre o modo de execução anterior (Thread ou Handler), a pilha a ser usada e se a restauração do estado do registrador xPSR (Program Status Register estendido) é necessária. Isso permite que o Cortex-M, ao retornar de uma interrupção, restaure o contexto anterior e continue a execução a partir do ponto onde foi interrompido.

### (k) Qual  a  diferença  no  salvamento  de  contexto,  durante  a  chegada  de  uma  interrupção,  entre  os processadores Cortex-M3 e Cortex M4F (com ponto flutuante)? Descreva em termos de tempo e também de uso da pilha. Explique também o que é ***lazy stack*** e como ele é configurado. 

Em processadores Cortex-M3, o salvamento de contexto durante uma interrupção é mais manual, exigindo que o software manipule explicitamente os registradores de ponto flutuante, consumindo mais tempo e pilha. No Cortex-M4F, que possui unidade de ponto flutuante, o salvamento de contexto é mais eficiente e automático, reduzindo o tempo de tratamento da interrupção e utilizando menos espaço na pilha. O conceito de "lazy stack" permite economizar recursos ao empilhar somente os registradores necessários no momento da interrupção, otimizando o uso da pilha.
 

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