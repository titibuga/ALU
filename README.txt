RELATÓRIO DO EP2 DE MAC329 - ÁLGEBRA BOOLEANA E SUAS APLICAÇÕES
PRIMEIRO SEMESTRE DE 2013
PROFESSOR: JÚNIOR BARRERA

GRUPO:

-GERVÁSIO PROTÁSIO DOS SANTOS NETO , NUSP: 7990996
-MATEUS BARROS RODRIGUES, NUSP: 7991037
-VICTOR SANCHES PORTELLA, NUSP: 7991152
-VINÍCIUS JORGE VENDRAMINI: NUSP: 7991103

EP2: Arithmetic and Logic Unit

Na implementação da ALU foram utilizados varios subcircuitos para facilitar a vizualição e evitar que o circuito se torna-se demasiadamente complicado. A seguir, seram explicados estes subcircuitos:

   *half Adder: circuito que implementa a soma de dois bits, tendo como output o resultado e o bit de carry
   *Full Adder: circuito que soma dois bita e leva em conta o bit de carry no calculo da soma. Tem como output o resultado e o novo bit de carry. Faz uso do half adder.
   *Adder 8 bits: circuito que implementa a soma de numeros de 8 bits, fazendo uso do Full Adder. Para determinar a ocorrencia ou não de overflow, usou-se a seguinte lógica: o overflow aconteceria se, e somente se, quando somando dois números positivos o resultado fosse negativo ou se somando dois números negativos o resultado fosse positivo. Vale ressaltar que foi utilizado o complemento de 2 para representar números negativos.
   *Two's complement: circuito que calcula o complento de dois de um número, o seja, calcúla o oposto daquele número. Por exemplo, se seu input for 00010100 (20, em decimal), seu output sera 11101100 (236, em decimal. Nota-se que 236 = 2^8 - 20).
   *Subtraction: circuito que implementa a subtração. Reduziu-se a subtração a uma adição. O primeiro termo permanece inalterado, enquanto o segundo termo é transformado em seu complemento de dois (seu oposto, o número negativo correspondente a ele). Em posse dos dois números, faz-se a soma deles. Essa soma será a subtração entre os números originais e é o output do circuito.
   *Quarter-multiplier: este circuito implementa a primeira fase da multiplicação. Ele multiplica um número de 8 bits por um único bit. Ele possui dois outputs, ambos são números de 8 bits. Se estamos calculando AxB, o primeiro output será um shift do valor B e o segundo output será o próprio valor B (ainda não shiftado) se o bit multiplicado for 1 e será 00000000 se o bit for 0.
   *Full-multiplier 8 bits: Esse circuito implementa a multiplicação. Para realizar a operação AxB faz-se o seguinte:
   Passa-se b e o bit menos significativo de A como argumentos do quarter-multiplier. O resultado da multiplicação é armazenado em r0. Em seguida passa-se o número B shiftado (o outro output do quarter-adder) e o segundo bit menos significativo de A para outro quarter-multiplier. Repete-se esse processo até ter-se multiplicado o número B shiftado 7 vezes pelo bit mais significativo de A.
   Então, soma-se os resultados parciais, dois a dois, e o resultado da última soma será o resultado da multiplicação.
   *Módulo: Calcula o módulo de um número. Ou seja, se o número for positivo o output é o próprio número; se o número for negativo é devolvido seu complemento de dois (que será aquele número positivo).
   *Final Multiplier: Circuito final a ser usado para implementar a multiplicação. Primeiro computa-se o módulo de cada um dos números envolvidos na multiplicação. Então, usa-se os módulos como input do cicuito de multiplicação (Full-multiplier 8 bits). Por fim, para decidir se o resultado final da operação deve ser positivo ou negativo, faz-se um shor entre os bits mais significativos dos números originais (bit iguais, significam um output positivo, bits diferentes um output negativo).
   *Divisão: A divisão foi implementada como uma subtração interada.
    Esse circuito implementa a divisão de dois números (positivos). Neste cicuito fazemos uso de um clock e de dois registradores (um registrador e um contador).
   São usadas condições lógicas para fazer com que o clock não tenha efeito se o divisor for zero (de forma a impedir divisão por zero) ou se o conteúdo do registrador principal for negativo (o que indicaria o término da divisão).
   Primariamente, a entrada (o dividendo) é passa para o registrador. Isso só ocorre uma vez, os demais valores do registrador virarm do circuito de subtração. Para selecionar de onde deve vir o valor do registrador, utiliza-se um multiplexer. O bit de seleção do multiplexer será um OR entre os bits do contador, ou seja só será zero no primeiro ciclo do clock, isso assegura que o valor de entrada só vai uma vez para o registrador.
   Após o resgistrador ter se tornado negativo, a operação é considerada concluída. O quociente + 2 está presente no contador. O motivo de o valor no contador ser dois maior que o quociente é que um clock do relógio (que aumenta um no contador) é necessário para mandar o dividendo para o registrador e um clock é necessário para que o valor negativo do registrador seja atualizado. Logo, além de um número de clocks iguais ao quociente, tem-se 2 a mais, um para transferencia de valor e um para interromper a operação.
   Por fim, subtrai-se dois do valor guardado no contador e esse é o valor do output.
   *Final Divider: Circuito final para implementação da divisão. Funciona de forma similar ao Final Multiplier. Já que o circuito divisão foi concepicionado para lidar com números positivos, primeiro extrai-se o módulo dos números envolvidos, realiza-se a operção e por fim verifica-se se o output deve ser positivo ou negativo e o valor cabível é o output.
   *Resto: Este circuito calcula o resto da divisão entre dois números pela definição se temos dois números A e B. Então (pelo algoritmo da divisão) A = B*Q + R, onde Q é o quociente da divisão inteira e R é o resto da divisão.
 Temos então: R = A - B * (A / B).
 Então, primeiro calcula-se o quociente de A por B, então multiplica-se esse quociente por B e subtrai-se esse resultado de A. Optamos no design por retornar sempre o resto positivo; para isso passamos o resto por um operador que devolve o módulo dele, e esse resultado será o output.

 *main: Circuito principal do EP2, implementado a ALU em si. Nele há pinos para seleção de A, B e S (que definirá a operação a ser realizada). Passamos A e B como input para o circuito que realiza cada operação e guarda-se o resultado. Então, os resultados das operações são passados como entrada para um multiplexer, cuja saída é determinada por S, na convenção estabelecida no enunciado. A saída do multiplexer é então usada para definir os bits do resultado final. 
 Há também flags que indicam se houve overflow em alguma operação.
