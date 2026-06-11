# OPA Suite - Mapa Mental e Documentação Técnica


```text
OPA SUITE
│
├── COBRANÇA
│   │
│   ├── Dashboard
│   │   ├── Atendimentos do Dia
│   │   ├── Clientes em Atendimento
│   │   ├── Renegociações
│   │   └── Pagamentos Confirmados
│   │
│   ├── WhatsApp
│   │   ├── Templates
│   │   ├── Disparos em Massa
│   │   ├── Mensagens Automáticas
│   │   └── Atendimento Humano
│   │
│   ├── Chatbot
│   │   ├── Segunda Via
│   │   ├── Pix
│   │   ├── Consulta CPF
│   │   ├── Renegociação
│   │   └── Encaminhar Atendente
│   │
│   ├── Filas
│   │   ├── Novos Atendimentos
│   │   ├── Em Atendimento
│   │   ├── Aguardando Cliente
│   │   └── Finalizados
│   │
│   ├── Integração IXC
│   │   ├── Dados do Cliente
│   │   ├── Faturas
│   │   ├── Contratos
│   │   ├── Bloqueios
│   │   └── Reativações
│   │
│   ├── Operadores
│   │   ├── Supervisor
│   │   ├── Operador N2
│   │   └── Operador
│   │
│   └── Relatórios
│       ├── Clientes Atendidos
│       ├── Valores Recebidos
│       ├── Renegociações
│       ├── Reativações
│       └── Produtividade
│
├── ADMINISTRAÇÃO
│   ├── Usuários
│   ├── Permissões
│   ├── Templates
│   └── Configurações
│
└── INFRAESTRUTURA
    ├── WhatsApp Business API
    ├── Banco de Dados
    ├── API OPA
    ├── Logs
    └── Monitoramento
```
# **Fluxo de Interação - Base 1**
```text
┌─────────────────────────────┐
│ CLIENTE ENVIA MENSAGEM      │
│ VIA WHATSAPP                │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ WEBHOOK WHATSAPP API        │
│ Recebe a Mensagem           │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ CHATBOT SOLICITA CPF        │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ CLIENTE DIGITA CPF          │
└─────────────┬───────────────┘
              │
              ▼
┌─────────────────────────────┐
│ INTEGRAÇÃO IXC              │
│ Buscar Contrato             │
│ Buscar Status               │
│ Buscar Débitos              │
└─────────────┬───────────────┘
              │
      ┌───────┴────────┐
      │                │
      ▼                ▼
┌──────────────┐  ┌──────────────┐
│ POSSUI       │  │ NÃO POSSUI   │
│ DÉBITOS      │  │ DÉBITOS      │
└──────┬───────┘  └──────┬───────┘
       │                 │
       ▼                 ▼
┌─────────────────┐ ┌──────────────────┐
│ MENU DE OPÇÕES  │ │ Deseja falar com │
│                 │ │ um atendente?    │
│ [1] 2ª Via      │ └─────────┬────────┘
│ [2] Renegociar  │           │
│ [3] Atendente   │     ┌─────┴─────┐
└────────┬────────┘     │           │
         │              ▼           ▼
         │      ┌────────────┐ ┌─────────┐
         │      │ TRANSFERIR │ │ FINALIZA│
         │      │ PARA FILA  │ │ CHATBOT │
         │      └─────┬──────┘ └─────────┘
         │            │
         ▼            ▼
┌───────────────────────┐
│ CLIENTE ESCOLHE       │
│ RENEGOCIAR            │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│ PROPOSTA AUTOMÁTICA   │
│ Ex.: 10% desconto PIX │
└───────────┬───────────┘
            │
     ┌──────┴───────┐
     │              │
     ▼              ▼
┌───────────┐  ┌─────────────┐
│ ACEITOU   │  │ RECUSOU     │
└─────┬─────┘  └──────┬──────┘
      │               │
      ▼               ▼
┌──────────────┐ ┌───────────────┐
│ GERAR PIX    │ │ ENVIAR PARA   │
│ AUTOMÁTICO   │ │ ATENDENTE     │
└──────┬───────┘ └───────────────┘
       │
       ▼
┌─────────────────────┐
│ CLIENTE EFETUA      │
│ PAGAMENTO PIX       │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ GATEWAY PAGAMENTO   │
│ CONFIRMA PAGAMENTO  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ API OPA RECEBE      │
│ CONFIRMAÇÃO PIX     │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ IXC                 │
│ Baixa Faturas       │
│ Altera Status       │
│ Reativa Contrato    │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ CLIENTE RECEBE      │
│ "SINAL REATIVADO"   │
└─────────────────────┘
```
