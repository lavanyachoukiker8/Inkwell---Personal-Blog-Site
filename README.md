# 📝 Inkwell — Personal Blog Platform (UI Prototype)

A modern, minimal blog platform UI inspired by Medium, Hashnode, Dev.to, Ghost, and Notion — built as a single-file, front-end prototype with a full design system, mock data, and working client-side interactions.

![Status](https://img.shields.io/badge/status-prototype-yellow)
![HTML](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/license-MIT-green)

---

## ✨ Overview

Inkwell is a personal blogging platform concept with a clean, premium interface: rounded cards, soft shadows, a glassmorphism hero, full dark mode, and a signature emerald/sky-blue palette.

This repo currently contains a **fully interactive front-end prototype** — a single `index.html` file (HTML + CSS + vanilla JS, no build step) that demonstrates every core page and flow using in-memory mock data. It is not yet wired to a real backend/database — see [Roadmap](#-roadmap--planned-backend) for what's next.

**[🔗 Live Demo](#)** — replace with your GitHub Pages / Vercel / Netlify link once deployed (see [Deployment](#-deployment)).

---

## 📸 Preview

> Add screenshots or a screen recording here once deployed.

| Landing Page | Blog Feed | Editor |
|---|---|---|
| _screenshot_ | _screenshot_ | _screenshot_ |

---

## 🚀 Features

### Pages
- **Landing page** — hero with glassmorphism stat card, featured/trending/recent stories, category chips, author spotlight, testimonials, newsletter signup
- **Home / Feed** — live search, category filters, sort (recent / popular / most liked / reading time), pagination ("load more"), popular tags & top writers sidebar
- **Blog detail** — table of contents, reading-progress bar, like / bookmark / share, comments section, related articles
- **Editor** — Markdown toolbar (bold, italic, code, quote, list, link, image, heading), live preview pane, word count & reading-time calculator, SEO slug preview, AI title suggestion, autosave indicator
- **Dashboard** — stat cards (views, likes, comments, followers, bookmarks), weekly views chart, recent activity feed, posts table with status badges (published / draft / scheduled)
- **Profile** — editable name, bio, and social links (GitHub, LinkedIn, Twitter/X, website) via an edit-profile modal
- **Categories**, **About**, **Contact**, **Auth** (login / signup / Google button UI)

### Design system
- Palette: Emerald `#22C55E`, Sky Blue `#38BDF8`, Background `#F8FAFC`, full dark-mode theme via CSS variables
- Typography: **Poppins** (display) + **Inter** (body) + **JetBrains Mono** (code)
- Fully responsive (desktop, tablet, mobile)
- Toast notifications, skeleton-free instant interactions, keyboard-dismissible modal (`Esc`)

---

## 🛠 Tech Stack

**Current (prototype)**
- HTML5, CSS3 (custom properties, no framework), vanilla JavaScript
- Google Fonts (Poppins, Inter, JetBrains Mono)
- Zero build step — works by opening the file directly

**Planned (full-stack version)** — see [Roadmap](#-roadmap--planned-backend)
- Frontend: React.js + Tailwind CSS
- Backend: Django (or Flask) + Django REST Framework
- Database: PostgreSQL (production), SQLite (development)
- Auth: Django Auth + JWT + Google OAuth
- Storage: Cloudinary for media
- Deployment: Vercel (frontend) + Render/Railway (backend)

---

## 📂 Project Structure

```
inkwell/
├── index.html          # Full prototype (HTML + CSS + JS, single file)
├── README.md            # You are here
└── assets/               # (optional) screenshots, favicon, etc.
```

> When the backend is added, this repo will split into `frontend/` and `backend/` directories — see the roadmap section for the intended structure.

---

## ⚙️ Getting Started

No installation or build step is required for the current prototype.

### Option 1 — Just open it
1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/inkwell.git
   cd inkwell
   ```
2. Open `index.html` directly in your browser (double-click, or drag it into a browser window).

### Option 2 — Serve it locally (recommended for consistent behavior)
```bash
# Python 3
python -m http.server 8000

# or Node
npx serve .
```
Then visit `http://localhost:8000`.

---

## 🌐 Deployment

Since this is a static single HTML file, you can deploy it anywhere for free:

**GitHub Pages**
```bash
# in your repo settings → Pages → deploy from branch → main → / (root)
```

**Vercel / Netlify**
- Drag and drop the project folder into the Vercel/Netlify dashboard, or
- Connect your GitHub repo and deploy with zero configuration (static site, no build command needed).

---

## 🧭 Roadmap / Planned Backend

This prototype defines the target UI/UX and interaction design. The full production version is planned to include:

- [ ] Django REST API (Users, Blogs, Comments, Likes, Bookmarks, Followers, Categories, Tags, Notifications)
- [ ] PostgreSQL database with migrations
- [ ] JWT authentication + Google OAuth login
- [ ] Cloudinary integration for image/video uploads
- [ ] Real rich-text/Markdown editor with autosave to backend
- [ ] Admin panel (users, blogs, comments, reports, roles & permissions)
- [ ] SEO: sitemap.xml, robots.txt, RSS feed, Open Graph tags
- [ ] Analytics dashboard backed by real view/engagement data
- [ ] CI/CD deployment to Vercel (frontend) + Render/Railway (backend)

Contributions toward any of these are welcome — see [Contributing](#-contributing).

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 🙋 Author

**Lavanya** — [GitHub](#) · [LinkedIn](#) · [Twitter/X](#)

If you find this useful, consider giving the repo a ⭐!
