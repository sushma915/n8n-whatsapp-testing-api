# 🤖 WhatsApp AI Demo Bot

An AI-powered WhatsApp chatbot built using **n8n**, **WhatsApp Cloud API**, **OpenRouter**, and an **AI Agent**.

The bot receives messages from WhatsApp, processes them using an AI model, maintains short-term conversation memory, and sends the generated response back to the user.

## 🚀 Features

- 💬 Receive messages through WhatsApp
- 🤖 Generate AI-powered responses
- 🧠 Maintain recent conversation context
- 🔗 Use OpenRouter as the chat model provider
- ⚙️ Automate the complete workflow using n8n
- 📱 Send AI-generated responses back to WhatsApp
- 🗣️ Support natural and conversational replies

## 🏗️ Workflow Architecture

```text
📱 WhatsApp User
       │
       ▼
┌───────────────────┐
│  WhatsApp Trigger │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐
│     AI Agent      │◄──────────────┐
└─────────┬─────────┘               │
          │                         │
          ▼                         │
┌───────────────────┐     ┌────────────────┐
│   Send Message    │     │ OpenRouter     │
└─────────┬─────────┘     │ Chat Model     │
          │               └────────────────┘
          ▼
📱 WhatsApp User

Simple Memory ─────────────► AI Agent
```

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| **n8n** | Workflow automation |
| **WhatsApp Cloud API** | Receive and send WhatsApp messages |
| **OpenRouter** | AI chat model provider |
| **n8n AI Agent** | Understand messages and generate replies |
| **Simple Memory** | Maintain recent conversation context |

## 🔄 How It Works

### 1. WhatsApp Trigger

The workflow starts when a new WhatsApp message is received.

The WhatsApp Trigger listens for incoming `messages` events.

### 2. Extract User Message

The AI Agent receives the text from the incoming WhatsApp message:

```javascript
{{ $json.messages[0].text.body }}
```

### 3. AI Agent

The AI Agent processes the user's message and generates an appropriate response.

The configured instructions tell the agent to:

- Understand the user's message
- Give a clear and relevant response
- Keep the conversation natural
- Answer questions directly
- Ask only necessary follow-up questions
- Match the user's tone
- Keep responses concise unless detail is requested
- Avoid robotic language
- Provide actionable guidance when appropriate

### 4. OpenRouter Chat Model

The AI Agent uses OpenRouter as its chat model provider.

Configured model:

```text
openrouter/free
```

### 5. Conversation Memory

The workflow uses **Simple Memory** to retain recent conversation context.

The session is identified using the sender's WhatsApp number:

```javascript
{{ $json.messages[0].from }}
```

The configured context window is **15 messages**.

### 6. Send AI Response

The generated response is sent back to the WhatsApp user.

Recipient:

```javascript
{{ $('WhatsApp Trigger').item.json.messages[0].from }}
```

Message body:

```javascript
{{ $json.output }}
```

## 🔗 Workflow Connections

```text
WhatsApp Trigger
       │
       ▼
    AI Agent
       │
       ▼
 Send Message
```

Additional AI Agent connections:

```text
OpenRouter Chat Model ──► AI Agent
Simple Memory ───────────► AI Agent
```

## ⚙️ Setup

### Prerequisites

You need:

- An n8n instance
- WhatsApp Cloud API / WhatsApp Business setup
- OpenRouter account
- WhatsApp API credentials
- OpenRouter API credentials

### Import the Workflow

1. Open your n8n instance.
2. Go to **Workflows**.
3. Select **Import from File**.
4. Import `whatsapp-testing-api.json`.
5. Configure the WhatsApp credentials.
6. Configure the OpenRouter credentials.
7. Activate the workflow.
8. Send a WhatsApp message to the configured number.

## 🔐 Credentials

The workflow requires credentials for:

### WhatsApp

Used by:

- WhatsApp Trigger
- Send Message

### OpenRouter

Used by:

- OpenRouter Chat Model

## 🧠 Conversation Memory Example

```text
User: My name is Sushma.

Bot: Nice to meet you, Sushma! 👋

User: What programming language should I learn?

Bot: Since you're interested in technology, Python
would be a great choice...
```

The memory component allows the AI Agent to retain recent conversation context for the user's session.

## 👩‍💻 Author

**Sushma Thota**

Computer Science (AI) | Python | Generative AI | LangChain | AI Agents

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.
