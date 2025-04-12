# ProjectEva - Sistema de Assistente Virtual

## Visão Geral do Sistema

ProjectEva é um fork do Project Alice, um sistema de assistente virtual de código aberto para automação residencial e interação por voz. O sistema é projetado para funcionar localmente sem depender de serviços em nuvem, priorizando a privacidade e o controle do usuário.

## Arquitetura do Sistema

O sistema é construído com uma arquitetura modular baseada em Python, composta pelos seguintes componentes principais:

### Núcleo do Sistema

1. **Gerenciador de Inicialização (Initializer)**
   - Responsável pelo bootstrapping do sistema
   - Gerencia dependências e configurações iniciais

2. **SuperManager**
   - Gerencia todos os outros gerenciadores em uma estrutura hierárquica
   - Centraliza o acesso aos subsistemas

3. **Gerenciadores Especializados**
   - **ConfigManager**: Gerencia as configurações do sistema
   - **SkillManager**: Carrega e gerencia as habilidades instaladas
   - **DialogManager**: Gerencia o fluxo de diálogo e sessões de conversação
   - **ASRManager**: Gerencia o reconhecimento automático de fala
   - **TTSManager**: Gerencia a síntese de fala
   - **NLUManager**: Gerencia o entendimento de linguagem natural
   - **LLMManager**: Gerencia integração com modelos de linguagem grandes

### Entrada e Saída de Áudio

1. **ASR (Reconhecimento Automático de Fala)**
   - Módulos adaptáveis que convertam fala em texto
   - Suporte para múltiplos motores: Vosk, Coqui, etc.

2. **TTS (Síntese de Fala)**
   - Converte texto em fala
   - Suporte para múltiplos motores: Pico, Amazon, Google, etc.

3. **WakewordManager**
   - Detecta palavras de ativação para começar a escutar
   - Suporta diferentes motores de detecção: Porcupine, Precise, etc.

### Processamento de Linguagem Natural

1. **NLU (Entendimento de Linguagem Natural)**
   - Extrai intents e slots do texto do usuário
   - Implementações disponíveis:
     - **SnipsNLU**: Motor de NLU offline
     - **RasaNLU** (adicionado recentemente): Motor de NLU com recursos avançados e integração com spaCy

2. **LLM (Modelos de Linguagem Grandes)**
   - Integração com modelos avançados para conversação
   - Suporte para:
     - OpenAI (GPT)
     - Anthropic (Claude)
     - Google (Gemini)

### Interface Web e APIs

1. **WebUIManager**
   - Interface web para configuração e controle
   - Dashboard com widgets personalizáveis

2. **ApiManager**
   - API RESTful para integração com outros sistemas
   - Múltiplos endpoints para diferentes funcionalidades

## Skills (Habilidades) Instaladas

O sistema possui uma arquitetura baseada em skills independentes que podem ser instaladas e desativadas conforme necessário. As principais skills disponíveis incluem:

### 1. AliceCore
- Habilidade fundamental que gerencia os intents básicos do sistema
- Implementa funcionalidades essenciais e comandos de sistema
- Suporta múltiplos idiomas: inglês, francês, alemão, italiano, português, polonês

### 2. RedQueen
- Personalidade oficial do Project Alice
- Fornece respostas personalizadas e conversacionais
- Implementa comportamentos aleatórios para tornar o assistente mais natural

### 3. DateDayTimeYear
- Permite ao usuário perguntar sobre data, hora, dia da semana e ano
- Fornece informações temporais precisas
- Suporta vários formatos de solicitação de tempo

### 4. HomeAssistant
- Conecta o sistema ao Home Assistant para automação residencial
- Recursos:
  - Controle de interruptores, luzes e grupos de dispositivos
  - Captura e utiliza leituras de sensores (temperatura, umidade, etc.)
  - Atualização de estados de dispositivos a cada 5 minutos
  - Informações sobre nascer/pôr do sol, amanhecer/anoitecer

### 5. FacialRecognition
- Fornece funcionalidade de reconhecimento facial
- Recursos:
  - Monitora feeds de vídeo de múltiplos tipos de câmeras (USB, IP, RTSP)
  - Detecta e reconhece rostos registrados
  - Manipula múltiplos rostos em um único quadro
  - Registro de novos usuários em tempo real
  - Interface web para gerenciamento de rostos e câmeras

### 6. LlmConversation
- Permite interações avançadas com modelos de linguagem grandes
- Integra-se com provedores como OpenAI, Anthropic e Google
- Ativado por frases específicas como "Vamos conversar"
- Mantém contexto da conversa para diálogos coerentes

### 7. AliceSatellite
- Gerencia dispositivos satélites distribuídos pela casa
- Permite expandir a presença do assistente em múltiplos cômodos

### 8. ContextSensitive
- Adiciona consciência contextual às conversações
- Melhora a naturalidade das interações

### 9. Telegram
- Permite interação com o assistente através do aplicativo Telegram
- Recursos:
  - Comunicação bidirecional via chat
  - Gerenciamento de usuários autorizados
  - Controle remoto do sistema

## Componentes Recém-Integrados

### Sistema de NLU Rasa com spaCy
Foi implementada recentemente uma integração do Rasa NLU com spaCy para melhorar as capacidades de processamento de linguagem natural:

1. **Rasa NLU**:
   - Motor avançado de entendimento de linguagem natural
   - Suporte para fluxos de diálogo complexos
   - Gerenciamento de contexto de conversação

2. **spaCy**:
   - Análise linguística avançada
   - Tokenização precisa
   - Reconhecimento de entidades e modelos de linguagem pré-treinados
   - Especialmente útil para o idioma português

3. **Adaptadores**:
   - `SkillAdapter`: Compatibilidade entre o formato das skills existentes e o Rasa
   - `RasaDialogManager`: Gerenciamento avançado de diálogos

4. **Modo de Compatibilidade**:
   - O sistema pode funcionar mesmo sem o Rasa instalado
   - Implementação de modo de compatibilidade limitada para permitir operação básica

## Fluxo de Processamento

1. O sistema escuta constantemente pela palavra de ativação (wake word)
2. Quando ativado, o ASR captura e transcreve o áudio em texto
3. O texto é enviado ao NLU para extrair a intenção (intent) e slots (entidades)
4. Com base na intenção identificada, a skill apropriada é acionada
5. A skill processa a solicitação e gera uma resposta
6. A resposta é enviada ao TTS para ser convertida em fala
7. Quando necessário, comandos são enviados para sistemas externos (Home Assistant, etc.)

## Configuração e Personalização

O sistema é altamente configurável através de:
- Arquivo config.json para configurações gerais
- Arquivos de configuração específicos para cada skill
- Interface web para gerenciamento em tempo real
- Sistema de diálogos personalizáveis em múltiplos idiomas

## Considerações Técnicas

1. **Requisitos de Sistema**:
   - Python 3.10 ou superior
   - Dependências específicas para cada componente (ASR, TTS, NLU)
   - Hardware adequado para processamento de áudio e vídeo

2. **Desafios de Integração**:
   - Recentemente implementada a migração para Rasa+spaCy
   - Compatibilidade entre diferentes versões do Python
   - Gerenciamento de múltiplos processos (Telegram, reconhecimento facial)

3. **Privacidade e Segurança**:
   - Processamento local de fala e comandos
   - Armazenamento local de dados de reconhecimento facial
   - Apenas conexões externas quando absolutamente necessário (LLMs, atualizações)


