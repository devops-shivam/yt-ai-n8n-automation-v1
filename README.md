# 🎬 AI Reels Automation Workflow (n8n + Docker + FFmpeg)

This project automates the creation of Instagram Reels using AI-generated content. Built using **n8n**, it integrates with tools like **OpenAI**, **HuggingFace**, **Google Drive**, and **FFmpeg** to produce engaging video posts.

---

## 🚀 Features

- 💡 Generates “Did You Know?” content in Hinglish using AI
- 🖼️ Downloads and edits images from Google Drive
- 🤖 Calls HuggingFace API to generate visuals
- 📝 Adds overlays like fun facts and call-to-action text
- 🎵 Combines audio, image transitions, and text using FFmpeg
- ☁️ Uploads final reel to Google Drive

---

## 🐳 Prerequisites

- Docker and Docker Compose installed
- HuggingFace API key
- OpenAI API key
- Google Drive OAuth credentials
- A Linux-compatible system (for FFmpeg)

---

## 📁 Folder Structure

```
.
├── docker-compose.yml
├── AI_Reels_Template.json
├── reel_assets/
│   └── (audio/images go here)
└── n8n/
    └── (n8n data volume)
```

---

## ⚙️ Setup

### 1. Clone the Repo

```bash
git clone https://github.com/<your-username>/ai-reels-n8n.git
cd ai-reels-n8n
```

### 2. Create `docker-compose.yml`

```yaml
version: "3.7"

services:
  n8n:
    image: n8nio/n8n
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=securepassword
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - TZ=Asia/Kolkata
    volumes:
      - ./n8n:/home/node/.n8n
      - ./reel_assets:/home/node/n8n_storage/reel_audio
    entrypoint: >
      /bin/sh -c "
        apt-get update &&
        apt-get install -y ffmpeg &&
        n8n"
```

### 3. Run n8n in Docker

```bash
docker-compose up -d
```

---

## 🔑 Credentials Required in n8n

- **Google Drive OAuth2**
- **OpenAI API**
- **HTTP Header Auth** for HuggingFace

Configure these from **n8n → Settings → Credentials** after launching the instance.

---

## 📥 Import Workflow

1. Go to `http://localhost:5678`
2. Click **Import Workflow**
3. Upload the file: **AI_Reels_Template.json

---

## 🧠 How It Works

1. **Trigger:** Starts with a manual test trigger.
2. **AI Agent:** Generates Hinglish content with image prompts.
3. **Image Handling:** Downloads and composites Google Drive images.
4. **Text Overlay:** Adds Hinglish fact + CTA using fonts and coordinates.
5. **Audio + Video:** FFmpeg merges visuals and audio into a final `.mp4`.
6. **Storage:** Uploads final file back to Google Drive and local volume.

---

## 📝 Notes

- Customize FFmpeg command in the workflow node to modify video effects.
- Make sure font paths used in overlay nodes exist within the Docker container.
- You can automate this to run daily using the **cron node** in n8n.

---

## 📄 License

This project is licensed under the **MIT License**.

---

## 🙋 Need Help?

Feel free to open an issue
