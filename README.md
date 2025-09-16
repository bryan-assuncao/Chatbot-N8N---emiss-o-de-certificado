Claro\! Aqui está uma proposta de README para o seu projeto n8n.

-----

# Chatbot de WhatsApp para Emissão de Certificado Digital (n8n)

Este projeto é um fluxo de trabalho (workflow) básico construído na plataforma n8n para automatizar o atendimento inicial e a coleta de dados para a emissão de Certificados Digitais via WhatsApp.

O chatbot é projetado para guiar o usuário por um funil de vendas estruturado, desde a saudação inicial até a simulação de pagamento, utilizando o Google Gemini para a inteligência conversacional e a integração WAHA (WhatsApp HTTP API) para o envio e recebimento de mensagens.

## 🚀 Funcionalidades

  - **Fluxo de Conversa Guiado:** Conduz o cliente passo a passo na escolha do produto (e-CPF ou e-CNPJ), validade e tipo de certificado.
  - **Coleta de Dados Estruturada:** Solicita e armazena informações essenciais do cliente, como CPF/CNPJ, e-mail, telefone e endereço.
  - **Respostas Automáticas:** Simula a consulta de dados (como Razão Social e endereço a partir do CEP) para agilizar o processo.
  - **Memória de Conversa:** Utiliza o Redis para manter o contexto da conversa com cada usuário, permitindo um diálogo contínuo.
  - **Simulação de Pagamento:** Finaliza o fluxo com a apresentação de um valor e um código PIX fictício, simulando a confirmação do pagamento para concluir o atendimento.
  - **Validação de Entradas:** O agente é instruído a validar as respostas do usuário em cada etapa, repetindo a pergunta caso a resposta seja inválida.

## ⚙️ Como o Fluxo Funciona

O workflow é organizado da seguinte maneira:

1.  **Webhook:** O fluxo é iniciado quando uma nova mensagem do WhatsApp é recebida.
2.  **Set (Dados):** Os dados da mensagem recebida (como ID do chat, texto e nome do remetente) são extraídos e organizados para uso nas etapas seguintes.
3.  **Switch:** Um nó de controle verifica se o evento recebido é de fato uma "mensagem" antes de prosseguir, evitando que outros eventos (como status de entrega) acionem o chatbot.
4.  **AI Agent (Google Gemini):** Este é o cérebro do chatbot. Ele recebe a mensagem do usuário e segue um prompt detalhado que define todo o fluxo de conversa, as regras e as respostas esperadas em cada etapa.
5.  **Memory (Redis Chat Memory):** Armazena o histórico da conversa para que o agente de IA tenha contexto sobre o que já foi dito, evitando que o usuário precise repetir informações.
6.  **Send Seen:** Marca a mensagem do usuário como lida, melhorando a experiência e mostrando que o bot está "ativo".
7.  **Send a text message:** Envia a resposta gerada pelo AI Agent de volta para o usuário no WhatsApp.

## 🔧 Pré-requisitos

Para utilizar este workflow, você precisará de:

  - Uma instância do n8n funcionando.
  - Credenciais configuradas no n8n para:
      - **Google Gemini (ou PaLM API):** Para o modelo de linguagem.
      - **Redis:** Para a memória do chat.
      - **WAHA (WhatsApp HTTP API):** Para a conexão com o WhatsApp.

## 🚀 Como Usar

1.  **Importe o Workflow:** Faça o download do arquivo `Assistente AGR WhatsApp.json` e importe-o na sua instância n8n.
2.  **Configure as Credenciais:** Associe suas credenciais do Google Gemini, Redis e WAHA aos nós correspondentes no fluxo.
3.  **Ative o Workflow:** Salve e ative o workflow no canto superior direito da tela do n8n.
4.  **Configure o Webhook:** Configure sua instância WAHA para enviar eventos de novas mensagens para a URL do webhook de produção fornecida pelo n8n.
5.  **Inicie a Conversa:** Envie uma saudação (como "Olá") para o número de WhatsApp conectado e o chatbot iniciará o atendimento.
