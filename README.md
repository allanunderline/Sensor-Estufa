// Definindo os pinos dos componentes
const int pinSensorTemp = A0; // Pino do sensor de temperatura (TMP36) conectado ao pino analógico A0
const int pinMotor = 11;      // Pino do motor do ventilador conectado ao pino digital 11 (PWM)
const int pinLedR = 8;        // Pino do LED RGB (vermelho) conectado ao pino digital 8
const int pinLedG = 9;        // Pino do LED RGB (verde) conectado ao pino digital 9
const int pinLedB = 10;       // Pino do LED RGB (azul) conectado ao pino digital 10
const int pinBuzzer = 5;     // Pino da buzina conectado ao pino digital 13

void setup() {
  // Configuração dos pinos como saída e entrada
  pinMode(pinMotor, OUTPUT);  // Motor como saída
  pinMode(pinLedR, OUTPUT);   // LED vermelho como saída
  pinMode(pinLedG, OUTPUT);   // LED verde como saída
  pinMode(pinLedB, OUTPUT);   // LED azul como saída
  pinMode(pinBuzzer, OUTPUT); // Buzzer como saída
  pinMode(pinSensorTemp, INPUT); // Sensor de temperatura como entrada

  Serial.begin(9600); // Inicializa a comunicação serial para depuração
}

void loop() {
  // Leitura do valor analógico do sensor TMP36
  int leituraAnalogica = analogRead(pinSensorTemp);
  
  // Conversão da leitura analógica para temperatura em Celsius
  float voltagem = leituraAnalogica * (5.0 / 1023.0);
  float temperaturaCelsius = (voltagem - 0.5) * 100.0;

  // Imprime a temperatura no monitor serial
  Serial.print("Temperatura: ");
  Serial.print(temperaturaCelsius);
  Serial.println(" C");

  // Controle do LED e dos dispositivos com base na temperatura
  if (temperaturaCelsius >= 50) {
    // Situação de emergência: LED vermelho e buzina
    digitalWrite(pinLedR, HIGH);  // LED vermelho ligado
    digitalWrite(pinLedG, LOW);   // LED verde desligado
    digitalWrite(pinLedB, LOW);   // LED azul desligado
    digitalWrite(pinMotor, HIGH); // Motor ligado
    digitalWrite(pinBuzzer, HIGH); // Buzina ligada
  } else if (temperaturaCelsius >= 30) {
    // Temperatura moderada: LED azul e ventilador
    digitalWrite(pinLedR, LOW);   // LED vermelho desligado
    digitalWrite(pinLedG, HIGH);  // LED verde ligado
    digitalWrite(pinLedB, HIGH);  // LED azul ligado
    digitalWrite(pinMotor, HIGH); // Motor ligado
    digitalWrite(pinBuzzer, LOW);  // Buzina desligada
  } else {
    // Temperatura segura: LED verde
    digitalWrite(pinLedR, LOW);   // LED vermelho desligado
    digitalWrite(pinLedG, LOW);   // LED verde ligado
    digitalWrite(pinLedB, HIGH);  // LED azul desligado
    digitalWrite(pinMotor, LOW);  // Motor desligado
    digitalWrite(pinBuzzer, LOW); // Buzina desligada
  }

  delay(1000); // Atraso de 1 segundo antes da próxima leitura
}
