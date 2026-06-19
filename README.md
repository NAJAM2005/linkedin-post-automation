# 🤖 LinkedIn Post Automation (n8n + AI Agents)

An end-to-end agentic workflow that researches a topic, writes a LinkedIn post, finds a relevant image, sends it to me for human review via email, and automatically publishes the approved post to LinkedIn — all with zero manual posting.

---

## 🔍 How It Works

```
Form Trigger → Research Agent (Gemini) → Image Agent (Gemini) → Pexels Image Search
   → Download Image → Gmail Approval (Send & Wait) → LinkedIn Publish (API)
```

1. **Form Trigger** — I submit a topic and target audience
2. **Research Agent** — Gemini + Serper search tool researches the topic and drafts a LinkedIn-ready post (hook, body, CTA, hashtags)
3. **Image Agent** — Gemini generates a short visual description from the post
4. **Pexels Search** — fetches a real, topic-relevant stock photo using that description
5. **Gmail Approval** — sends me an email with the AI-written post and proposed image, and **waits** for my response
6. **Human-in-the-loop edit** — I can edit the post text directly in the approval form before approving
7. **LinkedIn Publish** — once approved, the post is automatically published to my LinkedIn profile via the LinkedIn API

---

## 🧠 Why Human-in-the-Loop?

Fully autonomous posting is risky — AI can hallucinate facts or miss tone. This workflow keeps a human checkpoint (via email) before anything goes live, while still automating 95% of the manual work: research, writing, and publishing.

---

## 🛠️ Tech Stack

- **n8n** — workflow orchestration (self-hosted, local)
- **Google Gemini** — LLM for research/writing + image prompt generation
- **Serper API** — real-time web search tool for the research agent
- **Pexels API** — free stock photo search
- **Gmail API** — approval workflow (Send and Wait node)
- **LinkedIn API** (`ugcPosts`) — direct REST API integration for publishing

---

## ⚙️ Setup

### Prerequisites
- [n8n](https://n8n.io) running locally or self-hosted
- A [Google Gemini API key](https://aistudio.google.com/)
- A [Serper API key](https://serper.dev)
- A [Pexels API key](https://www.pexels.com/api/)
- A [LinkedIn Developer App](https://developer.linkedin.com/) with `w_member_social` scope approved
- Gmail OAuth2 credentials connected in n8n

### Import
1. Import `LinkedIn_Automation.json` into n8n
2. Replace all placeholder values (`YOUR_PEXELS_API_KEY`, `YOUR_SERPER_API_KEY`, `YOUR_LINKEDIN_ACCESS_TOKEN`, `YOUR_PERSON_URN`, `YOUR_EMAIL@gmail.com`) with your own credentials
3. Connect your Gmail and Google Gemini credentials in their respective nodes
4. Activate the workflow and submit the form to test

---

## 🔑 Getting a LinkedIn Access Token

LinkedIn's API requires an OAuth2 app with the `w_member_social` scope:
1. Create an app at [developer.linkedin.com](https://developer.linkedin.com/apps)
2. Request the **"Share on LinkedIn"** product to unlock `w_member_social`
3. Generate a token via LinkedIn's [OAuth token generator](https://www.linkedin.com/developers/tools/oauth)
4. Find your **Person URN** via `https://api.linkedin.com/v2/userinfo`

---

## 🚧 Known Limitations / Roadmap

- ✅ Text posts publish automatically
- 🔜 **Image attachment to live post** — LinkedIn's image upload requires a 3-step API chain (register → upload binary → attach asset). The image is currently shown in the approval email for context but not yet attached to the published post. This is the next improvement.
- 🔜 Token refresh automation (currently tokens are generated manually and expire periodically)

---

## 👤 Author

**Najam Ul Hasan**
BSCS Student — Government College University Faisalabad
[GitHub](https://github.com/NAJAM2005) | [Email](mailto:najamhasan2000@gmail.com)
