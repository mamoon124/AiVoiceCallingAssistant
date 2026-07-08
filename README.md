# 🤖 AI Telephony Agent

A real-time AI-powered voice calling assistant that can **receive** and **place** phone calls over the Public Switched Telephone Network (PSTN). Built using **VideoSDK's Agents SDK**, **Twilio SIP Trunking**, and **Google Gemini Live API**, the agent enables natural, low-latency voice conversations without requiring a separate Speech-to-Text (STT) or Text-to-Speech (TTS) pipeline.

## ✨ Features

* 📞 Receive inbound phone calls automatically
* 📲 Place outbound calls through a simple API request
* 🎙️ Real-time speech-to-speech conversations
* ⚡ Low-latency audio streaming powered by Gemini Live
* ☎️ PSTN integration via Twilio SIP Trunking
* 🎧 Real-time media handling using VideoSDK Agents SDK
* 🔄 Supports multiple concurrent calls
* 🛠️ Easily customizable conversation flow

---

## 🏗️ Architecture

```text
Caller (PSTN)
      │
      ▼
 Twilio SIP Trunk
      │
      ▼
VideoSDK SIP Gateway
      │
      ▼
 VideoSDK Room
      │
      ▼
Python Agent (main.py)
      │
      ▼
Google Gemini Live API
```

### Component Overview

* **Twilio** manages the phone number and handles PSTN connectivity.
* **VideoSDK SIP Gateway** bridges Twilio SIP traffic into a VideoSDK room.
* **VideoSDK Agents SDK** runs the Python worker that joins the room and manages conversations.
* **Gemini Live API** provides native real-time speech-to-speech intelligence.

---

## 🛠️ Tech Stack

| Technology                 | Purpose                  |
| -------------------------- | ------------------------ |
| Python 3.10+               | Backend Agent            |
| VideoSDK Agents SDK        | Real-time communication  |
| Twilio                     | SIP Trunking & Telephony |
| Google Gemini Live API     | AI Voice Conversations   |
| Python Virtual Environment | Dependency Management    |

---

## 📂 Project Structure

```text
AiVoiceCallingAssistant/
├── main.py              # Agent entry point
├── requirements.txt     # Python dependencies
├── .env                 # Environment variables (ignored by Git)
├── .env.example         # Sample environment variables
└── README.md
```

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/AiVoiceCallingAssistant.git
cd AiVoiceCallingAssistant
```

### 2. Create a Virtual Environment

**Windows**

```bash
python -m venv .venv
.venv\Scripts\activate
```

**macOS / Linux**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 🔐 Environment Variables

Create a `.env` file in the project root.

```env
VIDEOSDK_AUTH_TOKEN=your_videosdk_auth_token
GOOGLE_API_KEY=your_google_gemini_api_key
```

---

## ▶️ Run the Agent

Start the AI agent locally:

```bash
python main.py
```

After startup, the worker registers itself with VideoSDK and waits for inbound or outbound call assignments.

---

# ☎️ Telephony Configuration

## Receiving Inbound Calls

### Step 1

Create an **Inbound Gateway** in the **VideoSDK Dashboard**.

Navigate to:

```text
Telephony → Inbound Gateways
```

Copy the generated Gateway URL.

### Step 2

Open the **Twilio Console**.

Navigate to your phone number's **Voice Configuration** and set the:

```
Origination SIP URI
```

to the VideoSDK Gateway URL.

### Step 3

Create a Routing Rule inside VideoSDK.

Configure:

* Gateway → Your Inbound Gateway
* Number → Your Twilio Number
* Dispatch Type → Agent (Self Hosted)
* Agent ID → Must match the `agent_id` inside `main.py`

Now every incoming phone call will be routed directly to your AI agent.

---

## Making Outbound Calls

Create an **Outbound Gateway** inside the VideoSDK Dashboard using your Twilio SIP Termination URI and credentials.

Create an outbound routing rule and invoke VideoSDK's SIP Call API.

Example:

```bash
curl -X POST https://api.videosdk.live/v2/sip/call \
-H "Authorization: YOUR_VIDEOSDK_TOKEN" \
-H "Content-Type: application/json" \
-d @body.json
```

Example `body.json`

```json
{
  "sipCallFrom": "+1XXXXXXXXXX",
  "sipCallTo": "+91XXXXXXXXXX",
  "routingRuleId": "your_routing_rule_id"
}
```

---

# ⚙️ Agent Configuration

The worker is configured in `main.py`.

Example:

```python
options = Options(
    agent_id="MyTelephonyAgent",
    register=True,
    max_processes=10,
    host="localhost",
    port=8081,
)
```

### Configuration Options

| Option          | Description                                                  |
| --------------- | ------------------------------------------------------------ |
| `agent_id`      | Must match the Agent ID configured in VideoSDK Routing Rules |
| `register`      | Registers the worker for telephony dispatch                  |
| `max_processes` | Maximum number of simultaneous calls                         |
| `host`          | Worker host                                                  |
| `port`          | Worker port                                                  |

The AI assistant's greeting, personality, and conversation flow can be customized by modifying the `on_enter()` method in your `Agent` subclass.

---

# 📋 Prerequisites

Before running the project, ensure you have:

* Python 3.10 or later
* A VideoSDK account with API credentials
* A Twilio account with a Voice-enabled phone number
* A Google AI Studio API key for Gemini Live

---

# 🐞 Troubleshooting

### Call disconnects immediately

Check the Twilio Call Logs for SIP response codes. This usually indicates an authentication, codec, or routing issue rather than a problem with the AI agent.

---

### No Twilio SIP logs

The request never reached Twilio.

Verify:

* Outbound Gateway credentials
* Termination SIP URI
* Routing Rule configuration

---

### Outbound phone never rings

Ensure:

* `sipCallFrom` is a valid Twilio Voice-enabled number.
* Destination numbers use the E.164 format (`+CountryCodeNumber`).
* The correct `routingRuleId` is used.

---

# 🔮 Future Improvements

* Multi-language conversations
* Conversation memory
* CRM integration
* Appointment scheduling
* Voice analytics dashboard
* Call recording support
* Emotion detection
* Custom AI personalities

---

# 🤝 Contributing

Contributions are welcome!

1. Fork the repository.
2. Create a new feature branch.
3. Commit your changes.
4. Push your branch.
5. Open a Pull Request.

---

# 📄 License

This project is licensed under the **MIT License**.

---

# 👨‍💻 Author

**Mamoon Khan**

If you found this project helpful, consider giving it a ⭐ on GitHub!
