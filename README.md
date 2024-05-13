# Projeto02_SED
A onda verde do Professor Kyller

Douglas de Souza Almeida - 118210378

Lucas Dantas Pereira - 118210176

## Vídeo Demonstração


[![Nome do Vídeo](https://img.youtube.com/vi/c7zKJGQ76oc/0.jpg)](https://www.youtube.com/embed/c7zKJGQ76oc)


## Descrição do Problema
  Considerando o trajeto entre o posto em frente à UFCG e o Rota Universitária mostrado na figura abaixo.
![image](https://github.com/douglaspicui1/Projeto02_SED/assets/166778388/aff3f38d-3708-420b-a9de-807ed81477fd)

  O objetivo deste projeto é modelar um sistema de semáforos usando UPPAAL, no qual o motorista vai atravessar quatro semáforos para chegar ao destino, garantindo que pare em no máximo um deles.

- Distância do posto ao 1º semáforo: 1.200m 
- Distância do 1º ao 2º semáforo: 130m
- Distância do 2º ao 3º semáforo: 160m
- Distância do 3º ao 4º semáforo: 170m
- Distância do 4º semáforo ao Rota: 90m
- Distância total: 1.750m
- Velocidade média de deslocamento: 40 km/h

<!--Foi incluído um fluxo de veículos e realiado a especificação do comportamento desejado através de fórmulas em lógica temporal e realizado a verificação delas. -->

## Modelo Utilizado

  No projeto, foi utilizado o UPPAAL para modelar o sistema de semáforos. UPPAAL é uma ferramenta de modelagem e verificação de sistemas temporizados, amplamente utilizada para sistemas embarcados e em tempo real. Ele permite a especificação de modelos de sistemas concorrentes, onde o comportamento é governado pelo tempo e por eventos discretos.

## Desenvolvimento

  Foram implementados quatro autômatos para os semáforos, um timer e um automato para o carro.
  
  As declarações globais incluem um canal de broadcast chamado "estado", utilizado para sincronizar o timer com os semáforos para troca de estados. Além disso, há a definição de um clock "time" para controlar o tempo do sistema. Foram utilizadas variáveis inteiras "A", "B", "C" e "D" para indicar o estado de cada semáforo (onde 0 indica verde ou amarelo e 1 indica vermelho).
 
  O vetor U[N_car] é utilizado para determinar a posição do carro em relação aos semáforos, representando o tempo mínimo necessário para o carro alcançar cada semáforo. Adicionalmente, foram definidas constantes inteiras "verde", "amarelo", "vermelho" e "temp", que indicam a duração de cada estado do semáforo, sendo  a constante "temp" utilizado para incrementar o tempo no sistema.

```c

// Declarações Globais
const int N_car=5;
typedef int [0,N_car-1] id_t;


broadcast chan estado;
clock time;
int A = 0, B=0, C=0, D=0;

int U[N_car];

const int verde = 30;
const int amarelo = 5;
const int vermelho = 20;
const int temp = 5;


```

Os quatro autômatos dos semáforos possuem uma estrutura similar, variando apenas nos estados iniciais. No caso, os semáforos "A" e "D" começam em verde, enquanto os outros começam em vermelho. Além disso, o valor do clock referente ao estado é atualizado e ocorre a sincronização com o timer por meio do canal utilizando a variável "estado". Para garatir o funcionamento adequado dos semáfaros e que o motorista independente do momento que ele chegue no primeiro semáfaro ele pare em apenas em um dos quatros semáfaros, foi implementado um atraso de 10 segundos de um semafáro para o outro, assim quando o primeiro semáfaro ficar verde, o semáfaro seguinte ficará verde 10 segundos após, no qual é o tempo aproximado de deslocamento para chegar no próximo semáfaro. Isso garante que independente do momento que o carro chegue ele irá pegar no máximo um semáfaro vermelho. 

![image](https://github.com/douglaspicui1/Projeto02_SED/assets/166778388/73bd2c09-1ec9-4e50-a15e-f45354d2145e)

No autômato do timer, existem duas formas de incremento. Uma delas ocorre a cada cinco unidades de tempo representados pela contante "temp", onde o incremento é realizado e em seguida verifica-se se o valor de "p" é igual a 0 para continuar essa ação. A outra forma é um incremento unitário, no qual se o usuário utilizar essa opção ele deve incrementar 5 unidades unitárias de tempo em sequência para garantir o funcionamento correto dos semáfaros e isso é controlado pela função "bloqueio()", que não permite que seja incrementado cinco unidades de tempo antes que as 5 unidades unitárias sejam finalizadas. Esse método é utilizado para incrementar o tempo permite que o usuário tenha um maior controle do momento que o carro irá atravessar o semáfaro, podendo ser qualquer número inteiro, contanto que o tempo mínimo para o carro chegar no semáfaro tenha sido satisfeito.

A função "test(int x)" incrementa o valor de tempo passado, para o vetor U[N_car] no qual o argumento x é o tempo escolhido, podendo ser 5 ou 1, e esse valor é incrementado em todos os carros . Isso é realizado em um loop que percorre de 0 a "N_car". A função "bloqueio()" incrementa o valor de "p" e, se "p" atingir 5, ele é resetado para 0.

![image](https://github.com/douglaspicui1/Projeto02_SED/assets/166778388/7e6d8269-eacd-461d-8fd8-874b0967aece)

```c
//Declarações do autômato timer
clock y;
int i;
int p=0;

void test(int x)
{
    for (i = 0; i < N_car; i++)
    {
        U[i] += x;
    }
}

void bloqueio()
{
    p++;
    if (p ==5){
        p=0;
    }
}
```

No autômato "Carro", são definidos seis estados: "Inicio", "Passou_do_Sem1", "Passou_do_Sem2", "Passou_do_Sem3", "Passou_do_Sem4" e "Fim". Cada estado está associado a uma condição de guarda, que considera as distâncias (convertidas para tempo) que o carro leva para chegar a cada semáforo ou ao final do percurso. Além disso, a passagem do carro é liberada apenas quando o semáforo correspondente estiver verde.

![image](https://github.com/douglaspicui1/Projeto02_SED/assets/166778388/64d2cd54-97a2-4409-8996-5cb22f834ef7)

```c
//Declarações autômato Carro
clock y;

const int d1 = 108;
const int d2 = 12; //11.7;
const int d3 = 15; //14.4;
const int d4 = 16; //15.3;
const int d5 = 9; //8.1;

``` 

Listamos um ou mais processos (ou instâncias de autômatos) que são compostos para formar o sistema principal do modelo. Aqui, os processos devem ser listados separados por vírgulas.

```c
// Place template instantiations here.


// List one or more processes to be composed into a system.
system Carro, SemaforoA, SemafaroB, SemafaroC, SemafaroD, tempo;

```

Os processos listados são "Carro", "SemaforoA", "SemafaroB", "SemafaroC", "SemafaroD" e "tempo", indicando que esses são os autômatos que compõem o sistema principal do modelo.

