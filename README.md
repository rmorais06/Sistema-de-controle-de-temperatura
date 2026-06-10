# Sistema-de-controle-de-temperatura
Aqui está uma sugestão de estrutura em Markdown para o README.md do seu repositório no GitHub. Você pode copiar o conteúdo abaixo e colar diretamente no arquivo do seu projeto.

Sistema de Controle de Temperatura em Malha Fechada 🌡️
Este repositório contém o firmware base desenvolvido para o projeto acadêmico de um sistema de controle térmico. O código foi projetado para operar em um Arduino Uno, realizando a aquisição de dados analógicos de um sensor de temperatura, conversão de grandezas elétricas e atuação sobre uma carga térmica (lâmpada), incluindo uma trava de segurança temporizada.

📋 Descrição do Projeto
O código implementa uma lógica sequencial cíclica que:

Aciona o atuador térmico em potência máxima (100%) logo na inicialização.

Lê continuamente os dados analógicos do sensor e converte a escala de tensão para graus Celsius.

Transmite os dados de telemetria em tempo real via comunicação serial.

Possui um mecanismo de proteção fail-safe não bloqueante (utilizando a função millis()) que desliga permanentemente o atuador após 15 segundos de operação contínua, prevenindo superaquecimento na bancada de testes.

🛠️ Componentes de Hardware Integrados
A lógica deste código foi desenvolvida para operar em conjunto com a seguinte arquitetura física:

Microcontrolador: Arduino Uno

Sensor de Temperatura: LM35 (Linear, 10mV/°C)

Atuador Térmico: Lâmpada Incandescente 12V 2W

Chaveamento: Transistor NPN 2N2222 (acionamento do estágio de potência)

🔌 Mapeamento de Pinos (Pinout)
Componente / Função	Pino no Arduino	Tipo de Sinal
Sensor LM35 (Sinal / VOUT)	A0	Entrada Analógica (ADC 10-bits)
Base do Transistor (Lâmpada)	9	Saída Digital
💻 Entendendo a Lógica de Conversão (ADC para °C)
O Arduino Uno possui um ADC de 10 bits, mapeando a tensão de 0V a 5V em valores inteiros de 0 a 1023. O código realiza o seguinte tratamento de dados:

Cálculo da Tensão: Valor ADC * (5.0 / 1024.0)

Cálculo da Temperatura: Sabendo que o LM35 varia 10mV (0.01V) a cada 1°C, basta dividir a tensão encontrada por 0.01. A resolução teórica do sistema é de aproximadamente 0,488 °C.

🚀 Como Executar
Realize a montagem do circuito elétrico na protoboard, isolando a alimentação do microcontrolador (USB) do circuito de potência da lâmpada (Fonte 12V externa).

Conecte o Arduino Uno ao computador.

Abra o código na Arduino IDE.

Selecione a placa e a porta COM correspondente.

Compile e faça o upload (Ctrl + U).

Abra o Monitor Serial (Ctrl + Shift + M) e certifique-se de configurar a taxa de transmissão para 9600 baud.

Exemplo de Saída no Monitor Serial:
Plaintext
ADC: 51 | Tensao (V): 0.25 | Temp (C): 24.90
ADC: 51 | Tensao (V): 0.25 | Temp (C): 24.90
ADC: 52 | Tensao (V): 0.25 | Temp (C): 25.39
⚠️ Observações de Segurança
A trava implementada no bloco condicional if (millis() > 15000) visa proteger o circuito e o ambiente de ensaio. Em testes de validação contínua (ex: implementação futura de controle PID), esta trava em malha aberta deve ser removida ou substituída por limites dinâmicos de temperatura máxima lida pelo sensor.
