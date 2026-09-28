# 🤖 Jose Automates — Portfolio

**The portfolio of Jose Brache Garcia, Full‑Stack AI Automation Software Developer.**
A fast, bilingual, fully responsive static site whose contact form is wired to a real AI lead pipeline:
someone sends a message → it's validated, **classified by AI** (Hot / Warm / Cold), saved to Airtable, answered with a confirmation email in **their language**, and Jose gets a Telegram alert — in seconds, with zero backend to maintain.

🌐 **Live site:** [jose-automates.github.io](https://jose-automates.github.io)

---

## ✨ What this repo is

| | |
|---|---|
| 📄 **Front‑end** | Hand‑written HTML, CSS and vanilla JavaScript. No framework, no build step — served as‑is by GitHub Pages. |
| 🌎 **Bilingual** | Every text on the page switches between **English and Spanish** with one click (auto‑detected from the browser, remembered on the next visit). |
| 📱 **Responsive** | Designed desktop‑first and tuned section by section for tablet (≤ 900 px) and mobile (≤ 560 px). |
| 🤖 **Back‑end** | There isn't one. All the "backend" work (validation, AI classification, storage, emails, notifications) runs in an **n8n** workflow triggered by a webhook. |
| 🔌 **The glue** | The contact form in `index.html` posts to the n8n webhook; `index.js` validates, sends and shows the result in a styled popup. |

---

## 🧭 What's on the page

| Section | Highlights |
|---|---|
| 🦸 **Hero** | Headline, 6 key services, a 10‑skill grid that always matches the title's width, profile card + photo |
| 👋 **Who I Am** | Short bio + education card with the Valencia College A.S. diploma (click to enlarge) |
| 💼 **My Work** | 4 real projects, each with a live screenshot, skills, a **GitHub** button and a **Live Site** button |
| 🔗 **Find Me Online** | Fiverr · Upwork · Workana · LinkedIn · GitHub · Facebook · Reddit |
| ✉️ **Get In Touch** | Contact form connected to the AI lead pipeline |

### Featured projects

| Project | What it shows | Links |
|---|---|---|
| 🦷 **Bright Smile Dental** | Bilingual landing page + AI‑classified leads → Google Sheets + Telegram | [Repo](https://github.com/jose-automates/Bright-Smile-Dental) · [Live](https://jose-automates.github.io/Bright-Smile-Dental/) |
| 🎨 **Artist Portfolio Website** | Bootstrap portfolio + Airtable, Gmail and Telegram automation | [Repo](https://github.com/jose-automates/Artist-Portfolio-Website) · [Live](https://jose-automates.github.io/Artist-Portfolio-Website/) |
| 🏡 **Realtor Landing Page** | Luxury real‑estate landing page with automated lead capture | [Repo](https://github.com/jose-automates/Realtor-Landing-Page) · [Live](https://jose-automates.github.io/Realtor-Landing-Page/) |
| 🏋️ **Titan Strength Gym** | Conversion‑focused gym landing page | [Repo](https://github.com/jose-automates/Gym-Landing-Page) · [Live](https://jose-automates.github.io/Gym-Landing-Page/) |

---

## 🧩 Tech stack

### Front‑end

| Piece | Role |
|---|---|
| 🧱 **HTML5** | Semantic, accessible markup (labels, `aria-*`, alt texts) |
| 🎨 **CSS3** | Custom design system with CSS variables, Grid/Flexbox layouts, 3 breakpoints, animations that respect *reduce motion* |
| ⚡ **Vanilla JavaScript** | i18n engine (EN/ES), form validation, popup modals, layout helpers — no dependencies |
| 🔤 **Google Fonts** | **Space Grotesk** (text) · **Space Mono** (labels, code‑style accents) |
| 🐙 **GitHub Pages** | Hosting, straight from the `main` branch |

### Lead pipeline (n8n)

| Piece | Role |
|---|---|
| ⚙️ **n8n** | Runs the workflow `portfolio_contacto_leads` (webhook → validation → AI → integrations), self‑hosted |
| 🧠 **OpenAI** (`gpt-4.1-mini`) | Classifies every message: **urgency** (Hot/Warm/Cold), **intent** (Hire/Recruit/Collaborate/Other), a one‑line reason and a suggested reply tone |
| ♊ **Google Gemini** | Fallback model if OpenAI is unavailable |
| 📊 **Airtable** | Source of truth for every lead (name, email, message, language, labels, received date) |
| 📧 **Gmail** | Confirmation email to the sender, styled like this site, in **English or Spanish** based on their message |
| 💬 **Telegram** | Instant alert to Jose with the lead and its AI labels (🔥 Hot · 🌤️ Warm · ❄️ Cold) |

---

## 🔄 How the contact form works

```
Visitor clicks "Send"
   │  index.js validates the fields (same rules as the server)
   ▼
POST  →  n8n webhook
   │  1. Re-validates everything server-side (+ hidden anti-spam field)
   │  2. AI classifies the lead (OpenAI, Gemini as fallback)
   │  3. Detects the message language (English → EN, anything else → ES)
   │  4. In parallel:  Airtable row  ·  confirmation email  ·  Telegram alert
   ▼
Responds only AFTER the email is sent
   │
   ▼
Popup on the site: "Message Sent — a confirmation email was sent to you@…"
```

**Built to be robust:**

- ✅ **Validation on both sides** — names (letters only), real‑looking emails (`name@domain.ext`), and messages that block any kind of code (HTML, JS, SQL, shell…) while still allowing links.
- 🪤 **Honeypot anti‑spam** — a hidden field only bots fill in; they get a fake "OK" and nothing is processed.
- 🔒 **CORS‑safe** — a "simple" URL‑encoded request (no preflight); the webhook only shares its answer with this site and local previews.
- 🚫 **No double sends** — while a message is on its way the button is locked (spinner + "Sending…") and repeat clicks or Enter presses are ignored.
- 🔁 **Retries** — every external call (AI, Airtable, Gmail, Telegram) retries 3× automatically.
- 🛟 **Graceful fallbacks** — if the AI fails the lead is still saved with default labels; if Gmail fails the visitor is told honestly and Jose gets an alert.
- 🚨 **Error workflow** — any failure sends Jose a Telegram alert with the failing step, the error and a hint to fix it.
- 🪟 **No `alert()` boxes** — every message (errors, success, failure) is a popup modal in the site's own style.

---

## 📁 Repo structure

```
├── index.html              ← the whole page (all sections + contact form + popup)
├── styles.css              ← design system, layouts and breakpoints
├── index.js                ← EN/ES translations, form validation & sending, popups, layout helpers
├── README.md
├── .gitignore
└── images/
    ├── photo.png                ← profile photo
    ├── valencia-diploma.png     ← A.S. diploma (Education card)
    ├── favicon/                 ← favicon.ico · favicon-512.png · apple-touch-icon.png
    └── projects/                ← one screenshot per project (.webp)
```

> 💡 All the visible text lives in the `i18n` object at the top of `index.js` — edit the English (`en`) and Spanish (`es`) entries there.

---

## 🛠️ Editing the site

It's plain static HTML — edit and push:

```bash
git add .
git commit -m "Update portfolio copy"
git push origin main
```

GitHub Pages redeploys automatically in about a minute.

**Preview locally** (the contact form also works from here):

```bash
python -m http.server 5510
```

then open <http://localhost:5510>.

---

## 📮 Contact form endpoint

The form is **not** connected to Formspree, Netlify or any form service. It talks directly to the n8n webhook set in the form's `action` attribute in `index.html`. To point it somewhere else, change that URL — and add the new site's address to the webhook's allowed origins in n8n.

---

## 👤 About me

**Jose Brache Garcia** — Full‑Stack AI Automation Software Developer, Dominican Republic.
Associate in Science, Computer Programming and Analysis — Valencia College (2024).

Find me on [LinkedIn](https://www.linkedin.com/in/jose-brache-garcia/) · [Fiverr](https://www.fiverr.com/jose_codes?public_mode=true) · [Upwork](https://www.upwork.com/freelancers/~01a56229c98da68f05) · [Workana](https://www.workana.com/freelancer/0c0fa298e1f014c8165b9c6b580fdcee) · [GitHub](https://github.com/jose-automates)
