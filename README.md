# 🤖 LinkedIn Post Automation (n8n + AI Agents)

An end-to-end agentic workflow that researches a topic, writes a LinkedIn post, generates a relevant image, sends it to me for human review via email, and automatically publishes the approved post — with the image attached — to LinkedIn.

---

## 🔍 How It Works

```
Form Trigger → Research Agent (Gemini) → Image Agent (Gemini) → Pollinations.ai Image Generation
   → Gmail Approval (Send & Wait) → LinkedIn Register Upload → LinkedIn Upload Image (Binary) → LinkedIn Publish (API)
```

1. **Form Trigger** — I submit a topic and target audience
2. **Research Agent** — Gemini + Serper search tool researches the topic and drafts a LinkedIn-ready post (hook, body, CTA, hashtags)
3. **Image Agent** — Gemini generates a visual description from the post
4. **Pollinations.ai** — generates a real image from that description (Pexels stock search is also supported as an alternate source)
5. **Gmail Approval** — sends me an email with the AI-written post and generated image, and **waits** for my response
6. **Human-in-the-loop edit** — I can edit the post text directly in the approval form before approving
7. **LinkedIn Image Upload** — on approval, the image is registered, uploaded as raw binary, and referenced by its asset URN
8. **LinkedIn Publish** — the post, now with the image attached, is published to my LinkedIn profile via the LinkedIn API

---

## 🧠 Why Human-in-the-Loop?

Fully autonomous posting is risky — AI can hallucinate facts or miss tone. This workflow keeps a human checkpoint (via email) before anything goes live, while still automating the research, writing, image generation, and publishing.

---

## 🛠️ Tech Stack

- **n8n** — workflow orchestration (self-hosted, local)
- **Google Gemini** — LLM for research/writing + image prompt generation
- **Serper API** — real-time web search tool for the research agent
- **Pollinations.ai** — AI image generation (primary image source)
- **Pexels API** — free stock photo search (alternate image source)
- **Gmail API** — approval workflow (Send and Wait node)
- **LinkedIn API** (`ugcPosts` + Assets API) — direct REST API integration for publishing, including image upload

---

## ⚙️ Setup

### Prerequisites
- [n8n](https://n8n.io) running locally or self-hosted
- A [Google Gemini API key](https://aistudio.google.com/)
- A [Serper API key](https://serper.dev)
- A [Pexels API key](https://www.pexels.com/api/) (optional, alternate image source)
- A [LinkedIn Developer App](https://developer.linkedin.com/) with `w_member_social` scope approved
- Gmail OAuth2 credentials connected in n8n

### Import
1. Import `LinkedIn_Automation.json` into n8n
2. Replace all placeholder values (`YOUR_SERPER_API_KEY`, `YOUR_PEXELS_API_KEY`, `YOUR_LINKEDIN_ACCESS_TOKEN`, `YOUR_PERSON_URN`, `YOUR_EMAIL@gmail.com`) with your own credentials
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

## 🩹 The Hard Part: Image Uploads

LinkedIn's API doesn't accept an image URL or base64 string directly. It needs a three-step handshake:
1. **Register the upload** — `POST /v2/assets?action=registerUpload` returns an upload URL and asset URN
2. **Upload the raw binary** — `PUT` the actual image bytes to that upload URL
3. **Reference the asset** — include the returned asset URN in the final post payload

Getting step 2 right (raw binary, not JSON/base64) was the main blocker in building this — it's what separates a post that publishes text-only from one that actually goes live with an image attached.

---

## 🚧 Known Limitations / Roadmap

- ✅ Text posts publish automatically
- ✅ Image generation and attachment to the live post (Pollinations.ai + LinkedIn's 3-step upload chain)
- 🔜 Carousel image support (currently single-image posts only)
- 🔜 Token refresh automation (currently tokens are generated manually and expire periodically)
- 🔜 Fully automatic triggering (currently self-hosted and triggered manually)

---

## 👤 Author

**Najam Ul Hasan**
BSCS Student — Government College University Faisalabad
[GitHub](https://github.com/NAJAM2005) | [Email](mailto:najamhasan2000@gmail.com)
