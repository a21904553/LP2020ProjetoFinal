**UNIVERSIDADE LUSÓFONA DE HUMANIDADES E TECNOLOGIAS**

*Linguagens de Programação I - 2019/2020*

# Projecto Final: Auto-Lusófona

# 1. Introdução

## 1.1 Recomendações

Na resolução deste projecto deve ser utilizada a Linguagem de Programação C. Para além da correta implementação dos requisitos, tenha em conta os seguintes aspetos:
- O código apresentado deve ser bem indentado. 
- O código deve compilar sem erros ou *warnings* utilizando o *gcc* com as seguintes flags:
 `-Wall -Wextra -Wpedantic -std=c99 -Wvla`
- Tenha em atenção os nomes dados das variáveis, para que sejam indicadores daquilo que as mesmas vão conter.
- Evite o uso de constantes mágicas. 
- Evite duplicação de código. 
- Considere a implementação de funções para melhorar a legibilidade, evitar a duplicação e criar soluções mais genéricas.
- É proíbida a utilização de variáveis globais - i.e. variáveis declaradas fora de qualquer função.
- Este trabalho poderá ser realizado individualmente ou em grupos com o máximo de 2 alunos.

## 1.2 Motivação

Numa fábrica com uma linha de montagem convergem uma quantidade enorme de peças, ferramentas e pessoas para dar origem aos mais variados produtos. A orquestração de todos estes delicados fatores de forma precisa é o segredo que faz com que a linha de montagem funcione de forma perfeita.


## 1.3 Descrição do Problema

Neste projecto pretende-se que os alunos escrevam um programa capaz de ajudar na gestão de uma linha de montagem de carros. Designadamente no que toca à gestão do stock de peças, necessário para a montagem dos veículos, bem com do controlo de qualidade, necessário para garantir a segurança dos carros.

# 2 Implementação

O programa a implementar receberá como input duas listas com um número indeterminado de elementos (ou seja, um ficheiro de input poderá ter 100 elementos enquanto que outro ficheiro de input poderá ter 1000 elementos). Pelo que será **obrigatório o uso de memória dinâmica**, usando a instrução `malloc`, ou equivalente, para representar estes dados em memória, mais especificamente através de **listas ligadas**.

## 2.1 Estruturas de Dados

### 2.1.1 Inventário

O inventário guarda os dados de quais as peças disponíveis para serem usadas na produção de automóveis. Existem contudo vários tipos de peças com características distintas:
*   Motores são representados por:
    *   Número de série - Conjunto de 20 caracteres alfanuméricos
    *   Potência - número inteiro que representa a potência do motor
    *   Tipo de Combustível - Poderá ser Gasolina, Gasóleo, Híbrido ou Elétrico
*   Chassis
    *   Número de série - Conjunto de 20 caracteres alfanuméricos
    *   Cor - Enumerado os seguintes valores: Vermelho, Verde, Azul, Amarelo, Preto, Branco, Cinzento
    *   Modelo - string com no máximo 127 caracteres
*   Jantes
    *   Número de Série - Conjunto de 20 caracteres alfanuméricos
    *   Diâmetro - Valor inteiro do diâmetro da jante
    *   Largura - Valor inteiro da largura da jante
    *   Cor - Enumerado os seguintes valores: Vermelho, Verde, Azul, Amarelo, Preto, Branco, Cinzento
*   Pneus
    *   Número de Série - Conjunto de 20 caracteres alfanuméricos
    *   Diâmetro - Valor inteiro do diâmetro do pneu
    *   Largura - Valor inteiro do diâmetro do pneu
    *   Altura - Valor inteiro do diâmetro do pneu

Os números de série são únicos para cada peça. Quer isto dizer que, por exemplo, poderão existir vários motores com a mesma cilindrada e tipo de combustível, contudo estes terão sempre números de série diferentes. Isto faz com que a ordem pela qual se remove da lista de peças disponíveis seja relevante para o resultado final. **Deverá ser removida sempre a primeira peça referida no ficheiro de entrada.** Por outras palavras, implementando o modelo _first in, first out_.

### 2.1.2 Representação do Carro

Representação do carro guarda o número de pedido que especifica a sua construção, bem como os detalhes das peças a serem usadas na sua produção, incluindo número de série. No pedido, é possível que haja peças com parâmetros por especificar, por exemplo, no caso dos motores pode ser apenas especificado o tipo de combustível, não interessando qual a sua cilindrada. Neste caso qualquer motor com o tipo de combustível especificado pode servir para a construção do carro.

No processo de produção, o carro vem representado pelos seguintes atributos: 
*   Número do pedido
    *   Este é um código alfanumérico de 20 caracteres (números e/ou letras). Este código é seguido da caracterização das peças necessárias. 
*   Motor
    *   Número de série - Conjunto de 20 caracteres alfanuméricos
    *   Potência - número inteiro que representa a potência do motor
    *   Tipo de Combustível - Poderá ser Gasolina, Gasóleo, Híbrido ou Elétrico
*   Chassis
    *   Número de série - Conjunto de 20 caracteres alfanuméricos
    *   Cor - Enumerado os seguintes valores: Vermelho, Verde, Azul, Amarelo, Preto, Branco, Cinzento
    *   Modelo - string com no máximo 127 caracteres
*   Jantes
    *   Número de Série - Conjunto de 20 caracteres alfanuméricos
    *   Diâmetro - Valor inteiro do diâmetro da jante
    *   Largura - Valor inteiro da largura da jante
    *   Cor - Enumerado os seguintes valores: Vermelho, Verde, Azul, Amarelo, Preto, Branco, Cinzento
*   Pneus
    *   Número de Série - Conjunto de 20 caracteres alfanuméricos
    *   Diâmetro - Valor inteiro do diâmetro do pneu
    *   Largura - Valor inteiro do diâmetro do pneu
    *   Altura - Valor inteiro do diâmetro do pneu
*   Estado (é conveniente registar o estado do pedido): 
    *   PorAvaliar - por avaliar se pode ou não ser produzido o veículo;
    *   Produzido - veículo produzido, tendo sido retiradas as correspondentes peças do inventário; 
    *   NaoPossivelProduzir - pedido não satisfeito, pois não existem disponíveis todas as peças necessárias para produção do carro. 

## 2.2 Ficheiros de Input

### 2.2.1 Inventário

O ficheiro de inventário terá como extensão `".inventario"`. Quanto ao conteúdo, o ficheiro de inventário deverá apresentar a informação para cada peça da seguinte forma:

<table>
  <tr>
   <td><strong>Peça</strong>
   </td>
   <td><strong>Ordem</strong>
   </td>
  </tr>
  <tr>
   <td><code>Motor</code>
   </td>
   <td><code>Motor,&lt;NumeroDeSerieDoMotor>,&lt;Cilindrada>,&lt;TipoCombustivel></code>
   </td>
  </tr>
  <tr>
   <td><code>Chassis</code>
   </td>
   <td><code>Chassis,&lt;NumeroDeSerieDoChassis>,&lt;Cor>,&lt;Modelo></code>
   </td>
  </tr>
  <tr>
   <td><code>Jantes</code>
   </td>
   <td><code>Jantes,&lt;NumeroDeSerieDaJante>,&lt;Diametro>,&lt;Largura>,&lt;Cor></code>
   </td>
  </tr>
  <tr>
   <td><code>Pneus</code>
   </td>
   <td><code>Pneus,&lt;NumeroDeSerieDaRoda>,&lt;Diametro>,&lt;Largura>,&lt;Altura></code>
   </td>
  </tr>
</table>

Por sua vez, será apresentada uma peça por cada linha do ficheiro. 

Exemplo do conteúdo de um ficheiro de inventário:
```
Motor,52858123654997597522,2000,Diesel
Chassis,10912468347292828298,Azul,Minivan
Jantes,09573275234426356451,16,205,Amarelo
Motor,12312455,1600,Gasolina
Pneus,00523442635645109573,17,215,50
```

Não há garantia que as peças venham organizadas de alguma forma, ou seja, **não começam** por aparecer todos os motores, seguidos dos chassis, por exemplo. As peças poderão vir misturadas.


### 2.2.2 Ordens de Produção de Carros

O ficheiro com as ordens de produção terá como extensão `".pedidos"`. Contém uma série pedidos de produção de carros (um pedido/ordem por linha). As ordens de produção descrevem que peças devem se usadas para produzir uma determinada viatura. Uma ordem poderá ter:
*   uma especificação completa, isto é, com todas as características de cada peça, ou 
*   uma especificação parcial, caso em que na ordem não se especifiquem todas as características da peça (por exemplo apenas uma, ou até mesmo nenhuma). 

Em ambos os casos, deverá ser escolhida a primeira peça que corresponda aos critérios descritos, seguindo a ordem do ficheiro de inventário. Por exemplo, se na descrição da ordem de produção o único requisito especificado para o chassis for este ser vermelho, deverá ser usado o primeiro chassis vermelho no inventário, não interessando neste caso o modelo.

Cada ordem de produção começará sempre com um número de ordem, único para cada pedido à fábrica. Cada conjunto de características de uma peça será separada por ponto e vírgula “;”, e aparecerão pela ordem:
1. Motores
2. Chassis
3. Jante
4. Pneu

Os atributos de cada peça devem ser separados de vírgulas. Caso um atributo não seja especificado, o campo aparecerá em branco, mas as vírgulas permanecem.

Exemplo de conteúdo de um ficheiro de pedidos:
```
A422TCRE234VD3KJ349D;2000,;Vermelho,;17,200,Platinum;17,200,45;
3KJ349DA422TCRE234VD;,diesel;Azul,Minivan;15,145,Vermelho;15,145,45;
DA422TC3KJ349RE234VD;,;Vermelho,;,,;,,;
```
onde cada linha se encontra dividida de acordo com:
```
<NumeroDePedido>;<Cilindrada>,<TipoCombustivel>;<Cor>,<Modelo>;<Diametro>,<Largura>,<Cor>;<Diametro>,<Largura>,<Altura>;
```


## 2.3 Funcionamento do Programa

O programa é executado da seguinte forma:
```bash
$>./fabrica caso1 
```
Com esta instrução, o programa deve assumir que os ficheiros a abrir serão `caso1.pedidos` e `caso1.inventario`. Quando executado, os ficheiros devem ser lidos e os dados devem ser guardados nas estruturas adequadas. 

Alternativamente, pode ser executado da seguinte forma:
```bash
$>./fabrica
```
Neste caso os ficheiros terão de ser carregados através das opções 6 e 7 do menú antes de se iniciar a produção. Caso a produção seja iniciada sem carregar ficheiros deverá ser mostrada a mensagem apresentada para este caso na seção Avisos e Tratamento de Erros. De seguida, um menu deve ser apresentado tal como se ilustra em baixo.
```C
printf("**********Autolusofona***********\n");
printf("1. Iniciar producao\n");
printf("2. Mostrar inventario atual\n");
printf("3. Mostrar pedidos satisfeitos\n");
printf("4. Mostrar pedidos insatisfeitos\n");
printf("5. Escrever ficheiros atualizados\n");
printf("6. Ler ficheiro de inventario\n");
printf("7. Ler ficheiro de pedidos\n");
printf("8. Sair\n");
printf("*********************************\n");
```
Detalham-se em seguida as opções do menú.


### 2.3.1 Iniciar Produção

Ao correr o comando para iniciar a orodução, cada um dos pedidos de produção existentes na estrutura de dados deve ser avaliado de forma sequencial. É necessário garantir que todas as peças necessárias existem no inventário. De facto, poderão não existir no inventário da fábrica todas as peças necessárias para montar um veículo. Neste caso o veículo não é produzido, sendo o pedido "etiquetado" como `NaoPossivelProduzir`, as peças que iriam ser usadas para a sua montagem permanecendo no inventário. Apenas quando existem todas as peças necessárias o veículo é produzido, sendo etiquetado como `Produzido`; os itens utilizados são retirados do inventário e é registado, no pedido satisfeito, o número de série de cada peça e as características completas destas.

Os pedidos devem ser avaliados de forma sequencial, do primeiro ao último do ficheiro de entrada. Isto quer dizer que eventualmente pode haver peças necessárias para os primeiros veículos, mas para os últimos já não. Deverá ser registado o número de série de cada peça encontrada que satisfaz o requisito do pedido. Só no final de verificar se existem todas as peças é que deverá ser atualizado o estado de se é ou não possível fabricar o carro. Caso seja decidido fabricar o carro, as peças deverão ser removidas do inventário, sendo registado quais as características das peças não especificadas. 

Caso tenha sido corrido o programa sem especificar o nome dos ficheiros, deverá ser impressa uma mensagem adequada, tal como especificado na secção de avisos e tratamento de erros.

### 2.3.2 Mostrar Inventário Atual

Este comando deve mostrar o estado atual do inventário, ou seja, a listagem de todas as peças no mesmo formato do ficheiro de saída. 
Os elementos presentes no inventário deverão ser mostrados pela ordem relativa em que foram encontrados no ficheiro de input, devendo contudo aparecer organizados da seguinte forma:
*   Motores
*   Chassis
*   Jantes
*   Pneu

Caso este comando seja executado depois de correr o comando de produção, mostrará o mesmo que o ficheiro de output.
Caso tenha sido corrido o programa sem existir inventário (o programa foi executado sem especificar o nome dos ficheiros nem foi carregado um ficheiro de inventário), deverá ser impressa uma mensagem adequada, tal como especificado na secção de avisos e tratamento de erros.

### 2.3.3 Mostrar Pedidos Satisfeitos

Ao receber uma instrução para apresentar a lista de pedidos satisfeitos, deverão ser apresentados os números dos pedidos assim como as peças que foram encontradas para o carro em análise. Os dados deverão ser apresentados no ecrã no mesmo formato do ficheiro de saída correspondente. Caso ainda não tenha sido iniciada a produção, dará um aviso.

### 2.3.4 Mostrar Pedidos Insatisfeitos

Ao receber uma instrução para apresentar a lista de pedidos insatisfeitos, deverão ser apresentados os números dos pedidos assim como as peças que no instante em que o carro ia ser avaliado não foram encontradas para o carro em análise.  Os dados deverão ser apresentados no ecrã no mesmo formato do ficheiro de saída correspondente. Caso ainda não tenha sido iniciada a produção, dará um aviso.

### 2.3.5 Escrever Ficheiros Atualizados

Escrever ficheiros de output. Todos os ficheiros terão o mesmo nome dos ficheiros de entrada, mas com o sufixo `_out`, sendo o seu tipo diferenciado pela extensão:
*   Inventário Atualizado - com extensão `.inventario` (sem acento :))
*   Carros Produzidos - com extensão `.produzidos`
*   Carros Não Produzidos - com a extensão `.naoproduzidos` (sem til)

Por exemplo, se o ficheiro de entrada tiver como nome “caso1” então os ficheiros de saída serão: `caso1_out.inventario`, `caso1_out.produzidos`, `caso1_out.naoproduzidos`.

### 2.3.6 Ler Ficheiro de Inventário

Ao usar esta opção, o utilizador deverá especificar o nome de um ficheiro do tipo “inventario” com dados de novas peças a ser adicionadas à correspondente estrutura de dados. Deverá ser apresentada uma mensagem a pedir o nome do ficheiro de inventário:
```
Introduza o nome do ficheiro de inventario: 
```
Tal como na introdução dos ficheiros através do argumento do programa também, aqui é apenas necessário especificar apenas o nome do ficheiro sem a extensão, sendo esta adicionada de forma automática.

### 2.3.7 Ler Ficheiro de Pedidos

Ao usar esta opção, o utilizador deverá especificar o nome de um ficheiro do tipo “pedidos” com dados de novos pedidos a ser adicionados à correspondente estrutura de dados. Deverá ser apresentada uma mensagem a pedir o nome do ficheiro de pedidos:
```
Introduza o nome do ficheiro de pedidos: 
```
O utilizador irá introduzir o nome do ficheiro que quer utilizar sem a extensão, devendo esta ser automaticamente adicionada.

### 2.3.8 Sair

Esta opção deverá fazer com que o programa termine sem crashar ou vazar memória.

## 2.4 Ficheiros de Output

### 2.4.1 Inventário Actualizado

O inventário atualizado segue o mesmo formato do ficheiro de inventário de input. Deverá ter o mesmo nome do ficheiro de entrada, mas com o sufixo “_out” (por exemplo, `caso1_out.inventario`). Os elementos presentes no inventário de output deverão ser mostrados pela ordem relativa em que foram encontrados no ficheiro de input, devendo contudo aparecer organizados da seguinte forma:
*   Motores
*   Chassis
*   Jantes
*   Pneu

### 2.4.2 Carros Produzidos

O ficheiro com os carros produzidos no fim de executar todas as ordens de produção possíveis deverá ter o mesmo nome do ficheiro de entrada, mas com o sufixo “_out” (por exemplo, `caso1_out.produzidos`). Deverá apresentar as características de cada unidade produzida da seguinte forma:
``` 
<NumeroDePedido>;<NumeroDeSerieDoMotor>,<Cilindrada>,<TipoCombustivel>;<NumeroDeSerieDoChassis>,<Cor>,<Modelo>;<NumeroDeSerieDaJante>,<Diametro>,<Largura>,<Cor>;<NumeroDeSerieDaRoda>,<Diametro>,<Largura>,<Altura>;
```
Cada carro produzido deverá produzir uma linha no ficheiro de saída.

No caso de existirem peças que não foram completamente especificadas no pedido devem aparecer com todos os detalhes das peças escolhidas no ficheiro de saída. Por exemplo, para a seguinte pedido no ficheiro de entrada:
```
DA422TC3KJ349RE234VD;,;Vermelho,;,,;,,;
```
Pode resultar num carro produzido com a seguinte especificação:
```
DA422TC3KJ349RE234VD;DALKSJDA12415152435,2000,Diesel;Vermelho,Coupe;17,200,Amarelo;17,200,45;
```

### 2.4.3 Carros Não Produzidos

O ficheiro com os carros não produzidos deverá ter o mesmo nome do ficheiro de entrada, mas com o sufixo “_out” (por exemplo, `caso1_out.naoproduzidos`). Deverá apresentar o número do pedido assim como as características das peças que não foram encontradas, deixando vazias as peças encontradas. Deverá respeitar a seguinte forma:
```
<NumeroDePedido>;<Cilindrada>,<TipoCombustivel>;<Cor><Modelo>;<Diametro><Largura><Cor>;<Diametro><Largura><Altura>;
A422TCRE234VD3KJ349D;,;Amarelo,;,,;,,;
```
No exemplo anterior, o pedido não foi satisfeito pois não foi encontrado um chassis amarelo. Cada pedido não satisfeito deverá produzir uma linha no ficheiro de saída.

## 2.5 Avisos e Tratamento de erros

Caso exista alguma inconsistência, deve ser criada uma mensagem de erro adequada e tomada uma ação específica: 
*   Caso não seja possível abrir o ficheiro o programa deverá terminar com a mensagem: ``Error: Could not open file.``
*   Caso algum ficheiro esteja mal formatado, o programa deverá terminar com a mensagem: ``Error: File is corrupted.``
*   Caso sejam introduzido um argumento inválido no menu, o programa deverá apresentar a mensagem ``Error: Invalid argument.``, e o programa deverá voltar a pedir a inserção de uma opção
*   Caso seja impossível alocar memória o programa deverá terminar com a mensagem de erro: ``Error: Couldn't allocate memory.``
*   Caso a produção não tenha sido ainda iniciada e seja pedida a lista de carros não produzidos o programa deverá apresentar a mensagem ``Warning: No production done initiated yet.``
*   Caso seja dada ordem de produção sem existir dados nas estruturas de dados (por não terem sido especificados ficheiros de entrada), o programa deverá apresentar a mensagem ``No files were specified.``

Em resumo a definição das mensagens de erro é:
```C
#define ERR_FOPEN "Error: could not open file."
#define ERR_CORPT "Error: File is corrupted."
#define ERR_ARGS "Error: Invalid argument."
#define ERR_MEM "Error: Couldn't allocate memory."
#define WARN_NO_PRODUCTION_INIT "Warning: No production done initiated yet."
#define ERR_NO_FILES "No files were specified"
```

## Material a entregar

* Ficheiro `.c` com código devidamente comentado e indentado:
    - Deve implementar as funcionalidades pedidas.
    - O código deverá ser submetido na plataforma PANDORA [(2)](#ref2) até às **23:59 do dia 12 de Julho de 2020** no *contest* **LP12020Projecto**.
    - A plataforma corre automaticamente uma série de testes e no fim atribui uma classificação **indicativa**. Os alunos deverão analisar o relatório emitido pela plataforma e poderão alterar o código e voltar a submeter o trabalho. Neste trabalho não haverá limite de submissões.
      A plataforma não permite a entrega de trabalhos após a data e hora limite.
* Ficheiro `relatorio_<nome_do_grupo>.pdf` em formato [PDF].
    - O relatório deverá ser submetido no moodle [(3)](#ref3) até às  **23:59 do dia 13 de Julho de 2020**. 
    - O nome do ficheiro *tem que respeitar o formato*. Por exemplo, se o nome do grupo for `king_mackerel`, o ficheiro deverá chamar-se `relatorio_king_mackerel.pdf`.
    - Incorrecta indentação do código poderá originar penalizações na nota.


## Relatório e Peer Review

Cada grupo deverá produzir e entregar um relatório cujo objectivo é explicar em detalhe a solução encontrada. Note que um relatório não deve conter código. Se for absolutamente essencial, pode, pontualmente, mostrar trechos de código.

O relatório deve conter *no mínimo* as seguintes secções:
   * Título
   * Descrição da solução
       * Fluxograma das funções principais
   * Conclusões e matéria aprendida
   * Referências
      * Incluindo trocas de ideias com colegas, código aberto reutilizado e bibliotecas utilizadas

Neste trabalho iremos aplicar uma técnica de *peer review* dos relatórios. Consequentemente os relatórios terão de ser anónimos - **os relatórios não podem conter os nomes, nem os números dos autores**. Apenas o nome do ficheiro terá de respeitar o formato: `relatorio_<nome_do_grupo>.pdf`. Ficheiros cujo nome não respeite o formato pedido ou que não estejam em formato PDF não serão aceites.

Cada aluno (individualmente) terá que rever e avaliar dois relatórios de colegas seus. Esta avaliação consiste no preenchimento de um ficheiro excel com critérios estipulados. Este ficheiro será disponibilizado na plataforma Moodle.

Após a submissão dos relatórios no Moodle, procederemos a uma distribuição dos relatórios pelos alunos. Os alunos deverão fazer a sua revisão e avaliação dos dois relatórios e submeter no Moodle o ficheiro excel preenchido.

Alunos que não submetam a sua avaliação dos relatórios do colegas, não terão nota no Mini Projecto.

## Peso na avaliação

O projecto vale 35% da nota final e será cotado de 0 a 20 valores. O projeto tem uma nota mínima de 8 valores para aprovação à UC. A avaliação do mesmo será feita da seguinte forma:

* 17  valores - Código (funcionalidade, indentação, comentários) - A classificação do Pandora servirá apenas de referência.
* 3 valores - Relatório.

## Honestidade Académica

Nesta disciplina, espera-se que cada aluno siga os mais altos padrões de honestidade académica. Trabalhos que sejam identificados como cópias serão anulados e os alunos envolvidos terão nota zero - quer tenham copiado, quer tenham deixado copiar.
Para evitar situações deste género, recomendamos aos alunos que nunca partilhem ou mostrem o seu código.
A decisão sobre se um trabalho é uma cópia cabe exclusivamente aos docentes da unidade curricular.
Os alunos são encorajados a discutir os problemas com outros alunos mas não deverão, no entanto, copiar códigos, documentação e relatórios de outros alunos. Em nenhuma circunstância deverão partilhar os seus próprios códigos, documentação e relatórios. De facto, não devem sequer deixar códigos, documentação e relatórios em computadores de uso partilhado.

## Referências

<a name="ref1"></a>

* (1) Pereira, A. (2017). C e Algoritmos, 2ª edição. Sílabo.

<a name="ref2"></a>

* (2)  PANDORA - Yet Another Automated Assessment Tool, https://saturn.ulusofona.pt/.

<a name="ref3"></a>

* (3)  Lusófona Moodle, https://secure.grupolusofona.pt/ulht/moodle/

<a name="ref3"></a>
* (3)  Conceito de Encriptação - [https://pt.wikipedia.org/wiki/Encripta%C3%A7%C3%A3o](https://pt.wikipedia.org/wiki/Encriptação)

## Metadados

* Autor: [Guilherme Sanches, Pedro Serra, Hugo Castro, Lucio Studer]
* Curso:  [Licenciatura em Engenharia Informática][lei]
* Curso:  [Licenciatura em Engenharia Informática e Redes de Telecomunicações][leirt]
* Curso:  [Licenciatura em Informática de Gestão][lig]
* Instituição: [Universidade Lusófona de Humanidades e Tecnologias][ULHT]
