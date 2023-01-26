# Enunciado do Projecto 2 - IAED 2021/2022

## Introdução

O objectivo deste projeto é o desenvolvimento, em linguagem C, de
funcionalidades adicionais às criadas no primeiro projeto.
A interacção com o programa deverá ocorrer através de um conjunto de
linhas compostas por uma letra (comando) e um número de argumentos
dependente do comando a executar.
Pode assumir que todo o *input* fornecido respeitará os tipos indicados,
por exemplo onde é esperado um valor inteiro decimal nunca será
introduzida uma letra.
Os comandos do primeiro projeto são listados na tabela seguinte e
mantêm as operações a executar e obedecem aos limites impostos,
exceto o limite superior de passageiros por voo que é eliminado.

| Comando | Acção |
|:---:|:---|
| __q__ | termina o programa |
| __a__ | adiciona um novo aeroporto ao sistema |
| __l__ | lista os aeroportos |
| __v__ | adiciona um voo ou lista todos os voos |
| __p__ | lista os voos com partida de um aeroporto |
| __c__ | lista os voos com chegada a um aeroporto |
| __t__ | avança a data do sistema |

Além dos comandos do primeiro projeto, são adicionados os
comandos listados na tabela seguinte, bem como as operações a executar.

| Comando | Acção |
|:---:|:---|
| __r__ | adiciona uma reserva ou lista as reservas de um voo |
| __e__ | elimina voos ou reserva |

## Especificação do problema

O objectivo do projecto é ter um sistema de gestão de voos comerciais
diários entre aeroportos.
Por forma a fazer a simulação da gestão dos voos consideramos
que o passar do tempo é controlado pela aplicação através de um comando que
permite definir a data actual do sistema. Quando o programa é iniciado
considera-se que a data actual é 1 de Janeiro de 2022.
O programa não vai ser testado com datas posteriores a 31 de Dezembro de 2023,
pelo que não vão ocorrer anos bissextos.

No sistema, considera-se que uma data no input ou output refere-se sempre
ao formato DD-MM-AAAA onde DD denota o dia (número entre 1 e o último
dia do mês), MM o mês (número entre 1 e 12) e AAAA o ano (número positivo).
As horas são também sempre no formato HH:MM onde HH denota a hora
(número entre 0 e 23) e MM denota os minutos (número entre 0 e 59).
Não é necessário validar se os dias, meses, horas, etc. estão dentro
dos intervalos válidos.

Nas situações em que o valor é inferior a 10, então o caracter das dezenas é 0.
Por exemplo, o dia 1 de Janeiro de 2022 é denotado por 01-01-2022
e as 9 horas e 5 minutos é representada por 09:05.
Finalmente, os períodos de tempo como a duração dos voos segue o formato das horas.

Cada __aeroporto__ é caracterizado por um identificador composto por
três letras maiusculas, um país (uma *string* não vazia com um máximo
de __30__ carateres) e uma cidade (uma *string* não vazia com um máximo
de __50__ carateres)

Note-se que na *string* que descreve a cidade onde o aeroporto está
localizado, podem ocorrer carateres brancos (espaços ou tabulador `\t`).
No entanto, a string que descreve o país apenas contém letras minusculas
ou maíusculas.
O sistema suporta no máximo __40__ aeroportos, sendo que os identificadores
dos aeroportos são únicos.

Cada __voo__ é caracterizado por um código de voo, o aeroporto de partida
e chegada, a data e hora de partida, a duração do voo e número máximo de passageiros.
O código de voo é uma *string* composta por duas letras maíusculas seguido
por um número entre 1 e 9999. Os números entre 1 e 9999 nunca se iniciam com um 0.
Podem existir vários voos com o mesmo código, mas não podem existir dois voos com
o mesmo código na mesma data de partida. Ou seja, voos com o mesmo código têm que
ocorrer sempre em dias diferentes.
O número máximo de passageiros pode ser qualquer inteiro entre 10 e 100 (inclusivé).
A duração de um voo nunca pode ser superior a 12 horas.
Podem existir no máximo __30000__ voos.

Não existem limites no número de reservas,
nem na dimensão dos códigos das reservas,
logo deve procurar utilizar a memória estritamente necessária.
Para facilitar a introdução dos dados, pode assumir que cada instrução não
excede 65535 carateres.
Se a memória se esgotar, o programa deve terminar de forma controlada,
imprimindo a mensagem `No memory.`
Antes de terminar, o programa deve libertar toda a memória reservada.

## Dados de Entrada

O programa deverá ler os dados de entrada a partir da linha de comandos do
terminal.

Durante a execução do programa as instruções devem ser lidas do terminal
(*standard input*) na forma de um conjunto de linhas iniciadas por um
carácter, que se passa a designar por comando, seguido de um número de
informações dependente do comando a executar; o comando e cada uma das
informações são separados por pelo menos um carácter branco.

Os comandos disponíveis são descritos de seguida. Os caracteres
`<` e `>` são utilizados apenas na descrição dos comandos para indicar os parametros.
Os parametros opcionais estão indicados entre caracteres `[` e `]`.
Cada comando tem sempre todos os parametros necessários à sua correcta execução.
Os caracteres `...` indicam possíveis repetições de um parametro.

Cada comando indica uma determinada acção que se passa a caracterizar em termos de formato
de entrada, formato de saída e erros a retornar. <u>Se o comando gerar mais
do que um erro, deverá ser indicado apenas o primeiro.</u>

  * __q__ - termina o programa:
    * Formato de entrada: `q`
    * Formato de saída: NADA

  * __a__ - adiciona um novo aeroporto ao sistema:
    * Formato de entrada: `a <IDAeroporto> <país> <cidade>`
    * Formato de saída: `airport <IDAeroporto>` onde `<IDAeroporto>` é o identificador do aeroporto criado.
    * Nota: a cidade pode conter caracteres brancos.
    * Erros:
        * `invalid airport ID` no caso do identificador de aeroporto não ser uma string apenas com maíusculas.
        * `too many airports` no caso de o aeroporto, se criado, exceder o limite máximo de aeroportos permitidos pelo sistema.
        * `duplicate airport` no caso de já existir um aeroporto com o mesmo identificador.

  * __l__ - lista os aeroportos:
    * Formato de entrada: `l [<IDAeroporto> <IDAeroporto> ...]`
    * Formato de saída: `<IDAeroporto> <cidade> <país> #voos` por cada aeroporto, um por linha onde #voos é o número de voos com origem no aeroporto.
        * Se o comando for invocado sem argumentos, todos os aeroportos são listados por ordem alfabética do código.
        * Se o comando for invocado com uma lista de identificadores de aeroportos, os aeroportos devem ser listados pela ordem dos respetivos identificadores no comando.
    * Erros:
        * `<IDAeroporto>: no such airport ID` no caso de não existir o aeroporto indicado.

  * __v__ - adiciona um voo ou lista todos os voos:
    * Formato de entrada: `v [<códigoVoo> <IDAeroportoPartida> <IDAeroportoChegada> <dataPartida> <horaPartida> <duração> <capacidade>]`
    * Formato de saída: `<códigoVoo> <IDAeroportoPartida> <IDAeroportoChegada> <dataPartida> <horaPartida>` por cada voo adicionado, pela ordem de criação ou nada, se for criado um novo voo.
    * Erros:
        * `invalid flight code` no caso do código do voo não ser uma string com duas maíusculas seguida por digitos (entre 1 a 4 digitos).
        * `flight already exists` no caso de já existir um voo com o mesmo código para o mesmo dia.
        * `<IDAeroporto>: no such airport ID` no caso de não existir o aeroporto (partida ou chegada) indicado.
        * `too many flights` no caso de o novo voo, a ser criado, exceda o limite de voos.
        * `invalid date` no caso de ser uma data no passado ou mais de um ano no futuro.
        * `invalid duration` no caso de ser um voo com duração superior a 12 horas.
        * `invalid capacity` no caso da capacidade não for um inteiro entre 10 e 100 (inclusivé).

  * __p__ - lista os voos com partida de um aeroporto:
    * Formato de entrada: `p <IDAeroporto>`
    * Formato de saída: `<códigoVoo> <IDAeroportoChegada> <dataPartida> <horaPartida>` por cada voo, um por linha ordenados por data e hora de partida. Os voos são ordenados da data e hora mais antigas para a mais recente. Num aeroporto não há dois voos com a mesma hora de partida no mesmo dia.
    * Erros:
        * `<IDAeroporto>: no such airport ID` no caso de não existir o aeroporto indicado.

  * __c__ - lista os voos com chegada a um aeroporto:
    * Formato de entrada: `c <IDAeroporto>`
    * Formato de saída: `<códigoVoo> <IDAeroportoPartida> <dataChegada> <horaChegada>` por cada voo, um por linha ordenados por data e hora de chegada. Os voos são ordenados da data e hora mais antigas para a mais recente. Num aeroporto não há dois voos com a mesma hora de chegada no mesmo dia.
    * Erros:
        * `<IDAeroporto>: no such airport ID` no caso de não existir o aeroporto indicado.

  * __t__ - avança a data do sistema:
    * Formato de entrada: `t <data>`
    * Formato de saída: `<data>` onde `<data>`é novo valor da data atual.
    * Erros:
        * `invalid date` no caso de ser uma data no passado ou mais de um ano no futuro.

  * __r__ - adiciona uma reserva ou lista as reserva de um voo:
    * Formato de entrada: `r <códigoVoo> <data> [<códigoReserva> <númeroPassageiros>]`
    * Formato de saída: `<códigoReserva> <númeroPassageiros>` para cada reserva no voo com o código `<códigoVoo>` na data `<data>`. Uma reserva por linha por ordem lexicográfica do código de reserva.
    * Erros:
        * `invalid reservation code` no caso do código da reserva não ser uma string composta apenas por maíusculas e digitos ou se for composto por menos de 10 caracteres.
        * `<códigoVoo>: flight does not exist` no caso de não existir um voo com o código na data indicada.
        * `<códigoReserva>: flight reservation already used` no caso de já existir uma reserva com o `<códigoReserva>` indicado.
        * `too many reservations` no caso da reserva, a ser criada, exceda a capacidade do voo.
        * `invalid date` no caso de ser uma data no passado ou mais de um ano no futuro.
        * `invalid passenger number` no caso do número de passageiros não seja um inteiro superior a 0.

  * __e__ - elimina voos ou reserva:
    * Formato de entrada: `e <código>`
    * Formato de saída: Apaga todos os voos ou a reserva com o `<código>` indicado. Caso se trate de um código de voo também devem ser eliminadas todas as reservas associadas aos voos removidos.
    * Erros:
        * `not found` no caso de não existir o `<código>`.


__Só poderá usar as funções de biblioteca definidas em `stdio.h`,
`stdlib.h`, `ctype.h` e `string.h`__

*Nota importante*: não é permitida a utilização da função `qsort` nativa do C e este *nome* não deve aparecer no vosso código.

O compilador a utilizar é o `gcc` com as seguintes opções de compilação:
`-Wall -Wextra -Werror -ansi -pedantic`. Para compilar o programa deve
executar o seguinte comando:

```
    $ gcc -Wall -Wextra -Werror -ansi -pedantic -o proj1 *.c
```

O programa deverá escrever no *standard output* as respostas aos comandos
apresentados no *standard input*. As respostas são igualmente linhas de
texto formatadas conforme definido anteriormente neste enunciado.
Tenha em atenção ao número de espaços entre elementos do seu output,
assim como a ausência de espaços no final de cada linha. Procure respeitar
escrupulosamente as indicações dadas.




