# Projeto02_SED
A onda verde do Professor Kyller

Douglas de Souza Almeida - 118210378

Lucas Dantas Pereira - 118210176

## Vídeo Demonstração

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
 
  As variáveis inteiras "aux1", "aux2" e "aux3" são utilizadas para determinar a posição do carro em relação aos semáforos, representando o tempo mínimo necessário para o carro alcançar cada semáforo. Adicionalmente, foram definidas constantes inteiras "verde", "amarelo", "vermelho" e "temp", que indicam a duração de cada estado do semáforo, sendo "temp" utilizado para incrementar o tempo no sistema.

```c

aaa

```

  
