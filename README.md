# Gmail AI Auto-Responder (n8n + LangChain + Google Gemini)

An open-source, production-ready n8n automation workflow that listens for incoming Gmail messages, intelligently evaluates whether they require a human response, drafts situational replies using Google Gemini, and drops them seamlessly into your Gmail thread as drafts for a human-in-the-loop review.

🚀 Features

- Real-Time Triggering: Monitors your Gmail inbox every minute for incoming unread emails (ignoring messages sent by you).
- Intelligent Noise Filtering: Employs LangChain and Gemini to identify marketing blasts, cold outreach, and general spam—automatically routing them out of the drafting engine.
- Context-Aware Drafting: Automatically translates logic constraints into professional, business-casual content matching the inbound sender's language.
- Conditional Multi-Drafting: If an incoming email presents a strict Yes/No or situational question, the LLM constructs *both* affirmative and negative options split by an intentional `------- OR -------` separator.
- Safe Human-in-the-Loop Delivery: Saves drafts directly within the active Gmail thread. Your emails never send automatically, eliminating the risk of unmonitored AI hallucinations.

🛠️ Tech Stack

- n8n: Visual node-based pipeline management.
- LangChain Node Integration: Handles structured JSON parsing and prompt conditioning.
- Google Gemini API: Leverages AI Studio developer free-tier keys for high-performance extraction and drafting without recurring operational costs.
- Gmail API (OAuth2): Handles target inbox message monitoring and direct injection of reply drafts.



🔧 Installation & Setup

1. Prerequisites
- A local or hosted instance of n8n (v1.x+ recommended).
- A free Google AI Studio account to grab a Gemini API key.
- A Google Cloud Console developer setup to generate Gmail OAuth2 Client Credentials.

2. Configure Google Cloud (Gmail API)
1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project and look up the Gmail API to click Enable.
3. Configure the OAuth Consent Screen as an *External* app type. Ensure you add your email address to the Test Users whitelist.
4. Go to the Credentials tab, choose Create Credentials -> **OAuth Client ID**, and set application type to Web application.
5. Add your n8n OAuth redirect URL (e.g., `http://localhost:5678/rest/oauth2-credential/callback`) under Authorized redirect URIs.
6. Keep the generated `Client ID` and `Client Secret` handy.

3. Configure Gemini Keys
1. Go to [Google AI Studio](https://aistudio.google.com/).
2. Click Create API Key and copy the string.

4. Import Workflow into n8n
1. Copy the raw JSON structure from `gmail-ai-auto-responder.json` in this repository.
2. Open your n8n canvas dashboard, press `Ctrl + V` (or `Cmd + V` on Mac) to paste the nodes directly into your workspace.
3. Open the Gmail Trigger and click Create New Credential to input your Google Cloud Client ID and Secret, then authenticate with your Google Account.
4. Open the Google Gemini Chat Model nodes and add your AI Studio API key under credentials.

🧪 Testing

1. Click Listen for test event on your workflow interface inside n8n.
2. From an external email address, send a message containing an actionable inquiry to your connected inbox (e.g., "Hey! Is our project launch sync still moving forward tomorrow morning?").
3. Watch the workflow execute along the `true` path.
4. Navigate to your Gmail interface, open the Drafts category, and verify your tailored situational draft.

