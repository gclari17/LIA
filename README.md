# Semáforo Acessível com Arduino

## Índice
1. [Concepção da Ideia](#concepcao-da-ideia)
2. [Resumo do Projeto](#resumo-do-projeto)
3. [Materiais Necessários](#materiais-necessarios)
4. [Como Montar](#como-montar)
5. [Código Necessário](#codigo-necessario)
6. [Testes Finais](#testes-finais)
7. [Análise dos Resultados](#analise-dos-resultados)

## Concepção da Ideia

A ideia de construir um semáforo com adaptação para cegos surgiu com a rotina diária. Por mais que alguns sinais da cidade já possuam esse mecanismo, infelizmente não são todos que têm essa característica. Já vi pessoas com deficiência visual precisando de auxílio para simplesmente atravessar uma rua, avenida, etc. Atualmente, mesmo com a presença de sinalizações visuais, há muita incidência de acidentes na capital. Acredito que, por mais simples que seja, o projeto contribui para uma melhora na infraestrutura da cidade em geral, prezando por ambientes mais seguros e inclusivos. Ao mudar os pequenos detalhes, grandes feitos são efetivados com o tempo. Além disso, o projeto evidencia a simplicidade de funcionamento, sendo mais um motivo para sua aplicação nas ruas.  

## Resumo do Projeto
Este projeto cria um semáforo acessível para pessoas cegas, com LEDs (verde, amarelo, vermelho) e um buzzer que toca diferentes melodias dependendo do estado do semáforo. O sistema alterna entre os estados a cada 4 segundos, proporcionando uma sinalização acessível e eficiente para quem não pode ver, mas pode identificar os sons.

## Materiais Necessários
Para montar este projeto, você vai precisar dos seguintes materiais:

- 1 Placa Arduino Uno (ou compatível)
- 1 LED Verde
- 1 LED Amarelo
- 1 LED Vermelho
- 1 Buzzer Passivo
- 1 Resistor de 220 ohms para cada LED
- 1 Resistor de 1k ohms para o buzzer
- Fios de conexão
- Protoboard (placa de ensaio)
- Fonte de alimentação para o Arduino (via USB ou adaptador)

## Como Montar
### Conecte os LEDs ao Arduino:
- Conecte o anodo (perna mais longa) de cada LED a um pino digital do Arduino:
  - **LED Verde** ao pino **3**
  - **LED Amarelo** ao pino **4**
  - **LED Vermelho** ao pino **5**
- Conecte o catodo (perna mais curta) de cada LED ao **GND** do Arduino, usando resistores de **220 ohms** em série.

### Conecte o Buzzer:
- Conecte o terminal positivo do buzzer ao pino **6** do Arduino.
- Conecte o terminal negativo do buzzer ao **GND** do Arduino, utilizando um resistor de **1k ohms**, se necessário.

### Fios e Protoboard:
- Organize os componentes na protoboard e faça as conexões com fios de forma que todos os pinos sejam conectados corretamente.

### Verifique as conexões:
- Garanta que todos os LEDs e o buzzer estão conectados corretamente ao Arduino.

## Código Necessário
```cpp
// Definição dos pinos dos LEDs e do Buzzer
const int ledVerde = 3;
const int ledAmarelo = 4;
const int ledVermelho = 5;
const int buzzer = 6;
int estadoSemaforo = 0;  // Estado do semáforo (0 = verde, 1 = amarelo, 2 = vermelho)
unsigned long tempoAnterior = 0;  // Variável para armazenar o tempo anterior
unsigned long intervalo = 4000;  // Tempo de duração de cada estado (4 segundos)
bool modoSom = true;  // Som ativado por padrão

void setup() {
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  pinMode(buzzer, OUTPUT);
}

void loop() {
  unsigned long tempoAtual = millis();  // Tempo atual

  // Verifica se é hora de mudar o estado do semáforo (a cada 4 segundos)
  if (tempoAtual - tempoAnterior >= intervalo) {
    tempoAnterior = tempoAtual;  // Atualiza o tempo anterior
    estadoSemaforo = (estadoSemaforo + 1) % 3;  // Muda o estado (0 -> 1 -> 2 -> 0)
  }

  // Atualiza os LEDs e sons de acordo com o estado do semáforo
  switch (estadoSemaforo) {
    case 0: // Verde
      controlarSemaforo(ledVerde);
      if (modoSom) tocarSomVerde();
      break;
    case 1: // Amarelo
      controlarSemaforo(ledAmarelo);
      if (modoSom) tocarSomAmarelo();
      break;
    case 2: // Vermelho
      controlarSemaforo(ledVermelho);
      if (modoSom) tocarSomVermelho();
      break;
  }

  // Desliga os LEDs após 4 segundos de acendimento
  if (tempoAtual - tempoAnterior >= intervalo) {
    desligarSemaforos();
  }
}

// Função para acender o LED ativo e apagar os outros
void controlarSemaforo(int ledAtivo) {
  digitalWrite(ledVerde, (ledAtivo == ledVerde) ? HIGH : LOW);
  digitalWrite(ledAmarelo, (ledAtivo == ledAmarelo) ? HIGH : LOW);
  digitalWrite(ledVermelho, (ledAtivo == ledVermelho) ? HIGH : LOW);
}

// Função para desligar todos os LEDs
void desligarSemaforos() {
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarelo, LOW);
  digitalWrite(ledVermelho, LOW);
}

// Sons personalizados para cada cor usando o buzzer passivo
void tocarSomVerde() {
  tone(buzzer, 440, 500); // Nota Lá
  delay(500);
  noTone(buzzer);
}

void tocarSomAmarelo() {
  tone(buzzer, 660, 300); // Nota Mi
  delay(300);
  tone(buzzer, 520, 300); // Nota Dó
  delay(300);
  noTone(buzzer);
}

void tocarSomVermelho() {
  tone(buzzer, 880, 200); // Nota Lá aguda
  delay(200);
  tone(buzzer, 700, 200); // Nota Sol
  delay(200);
  tone(buzzer, 500, 200); // Nota Dó grave
  delay(200);
  noTone(buzzer);
}
```

## Testes Finais

O projeto foi aplicado com facilidade. Houve tentativas de aprimoramento com acionamento manual, que permitiria acionar ou não o modo auditivo, e acréscimo de sensibilidade à luz para ser uma iniciativa mais sustentável (de acordo com a incidência de luz ambiente, o semáforo adaptaria seus níveis de luminosidade). Apesar das ideias serem boas, ou não foi possível aplicá-las, ou seus resultados não foram muito significativos para serem incluídos no projeto. Por outro lado, o funcionamento do mecanismo foi rapidamente efetuado, sem muitas dificuldades de adaptações e mudanças. É possível dizer que sua montagem é fácil e muitas dúvidas em relação ao código podem ser resolvidas pelo próprio código, que contém instruções e indicações.

## Análise dos Resultados

O semáforo com adaptação auditiva cumpriu as expectativas feitas na concepção do projeto. As melodias para cada cor são facilmente percebidas e diferenciadas, sendo isso de grande importância. O funcionamento técnico, como tempo de ativação, ciclo do programa, eficiência, entre outros aspectos estão dentro do esperado. Em relação a aprimoramentos, alguns podem ser considerados, tal como o acionamento manual, sensibilidade à luz, display de tempo (não como eficiente na inclusão à deficiências, mas como aplicação coerente do dia a dia), entre outros. Em suma, o projeto cumpre seu papel principal de inclusão, é eficiente e prático, mas existem possibilidades de aprimoramento.
