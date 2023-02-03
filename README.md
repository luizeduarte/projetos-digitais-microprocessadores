### Relatório da implementação do MICO X1 no Logisim Evolution.

   #### 1. Introdução.
O processador MICO X1 foi implementado através do simulador Logisim Evolution, utilizando apenas as categorias Wiring, Gates, Plexers e Memory. Ele é capaz de processar dados de 32 bits, realizando operações logicas e aritméticas, além de controle de fluxo. Cada instrução é composta de 4 bits para o código da operação, os três registradores que serão utilizados — dois para leitura e um para escrita — e uma constante de 16 bits. 

   #### 2. Somador.

   2.1 O formato do somador.

O somador utilizado na ULA é do tipo Carry Look-Ahead, formado por dois somadores em sequência de tal tipo para 16 bits, o qual é formado de dois andares de circuitos combinacionais da lógica Look-Ahead. O primeiro andar seria os somadores de 4-bits, onde cada um possui seu próprio circuito combinacional citado, que geram, dentre as suas saídas, as Gera e Propaga carry (GG e PG). Já o segundo andar, seria um circuito da lógica Look-Ahead ligado a 4 somadores menores, utilizando suas saídas GG e PG como entradas. O somador irá necessitar ao todo, portanto, de 8 somadores de 4-bit e 10 circuitos combinacionais da lógica Look-Ahead.
Tal escolha foi feita devido à sua eficiência na velocidade para realizar as operações — que será detalhada a seguir — apesar do espaço utilizado.

2.2 Tempo e espaço utilizado por outros somadores.

Considerando que a soma é feita com 32 bits e que cada porta lógica tem o atraso de 1 unidade de tempo, um somador Ripple Carry, por exemplo, teria um espaço e atraso de 32 Full-Adders para conseguir completar a soma, ou seja, 64 unidades de tempo. Já um somador do tipo Seletor pode diminuir pela metade o tempo de atraso quando comparado à um Ripple Carry. Contudo, cada vez que o atraso sofre tal alteração, o espaço utilizado aumenta em 50% na área alterada. 

2.3 Tempo e espaço no somador selecionado.

O tempo de atraso total utilizado pelo somador implementado é de apenas 16 unidades de tempo. Primeiramente, os quatro primeiros somadores de 1-bit levam uma unidade de tempo para produzir suas saídas Propaga e Gera carry. Logo após, a unidade lógica do primeiro andar do Look-Ahead, como descrito anteriormente, leva 2 unidades de tempo para gerar as suas saídas PG e GG. Tais saídas estão ligadas à uma nova unidade lógica do mesmo tipo, o segundo andar, o qual levará também 2 unidades de tempo para gerar suas saídas Carry-in, utilizadas por cada somador de 4-bits. Com seus respectivos Carry-in disponíveis, cada somador poderá gerar seus carrys em suas unidades lógicas no primeiro andar, levando 2 unidades de tempo. E, por fim, o resultado final é gerado com mais 1 unidade de tempo.

#### 3. Código de teste.
	
```
#include <stdio.h>
#define constante 2

int main(){
int a = 219;
int b = 199;

int c = a + b;
printf("%d", c );

int d = a - b;
printf("%d", d );

int e = a & b;
printf("%d", e );

int f = a | b;
printf("%d", f);

int g = a ^ b;
printf("%d", g );

int h = !a;
printf("%d", h );

int i = a << d;
printf("%d", i );

int j = b >> d;
printf("%d", j );

int k = a || constante;
printf("%d", k );

int l = a ^ constante;
printf("%d", l );

int m = a + constante;
printf("%d", m );

goto prox; //salto
int n = a + b; 
prox:
if (a == a) //desvio
    return 0;
int o  = a + b;
return 0;
}
```
