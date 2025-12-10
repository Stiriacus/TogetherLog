# TogetherLog

TogetherLog is a modern, elegant platform for couples and close groups to document shared memories as an interactive online flipbook.  
Upload photos, add highlights, assign tags, set locations, and TogetherLog automatically generates beautiful "Smart Pages" that feel like a digital scrapbook.

---

## 🚀 Features (Version 1)

- Create personal or shared memory logs  
- Upload photos (single or multiple)  
- Automatic extraction of EXIF data (date, GPS)  
- Reverse geocoding to detect location (editable by user)  
- Tag system with predefined categories  
- Highlight text per entry  
- Smart Pages (rule-based layout + color selection)  
- Flipbook viewer with page-turn animation  
- Emotion-inspired color themes  
- Data structure prepared for advanced layouts and decorations  

---

## 🔧 Planned (Future Versions)

- Map view using OpenStreetMap  
- Story slideshow mode  
- Decorative elements (sprinkles, polaroid frames, collage modes)  
- Heatmaps & analytics  
- Widget integration  
- Optional AI assistance for tagging, layout selection, and highlight suggestions  
- Gamification toggle for groups that enjoy progress tracking  

See the full optional feature archive in:  
**`v2optional.md`**

---

## 📂 Project Structure (recommended)

/backend
/api
/models
/services
/storage

/frontend
/src
/components
/pages
/hooks
/styles

/docs
v1Spez.md
v2optional.md


---

## 🛠 Technology Recommendations

**Frontend**  
- React or SvelteKit  
- TailwindCSS  
- Flipbook animation library or custom canvas-based animation  

**Backend**  
- Node.js (Express / Fastify) or Python (Django / FastAPI)  
- PostgreSQL for relational data  
- S3-compatible storage (MinIO, local S3, etc.)  

**Infrastructure**  
- Docker & Docker Compose  
- Reverse proxy (NGINX)  
- Optional: CDN for images

---

## 📝 Documentation

Core specifications:  
- **`v1Spez.md`** – MVP specification  
- **`v2optional.md`** – Optional and future features  

---

## 🤝 Contributing

Contributions, feature suggestions, and pull requests are welcome.  
This project aims to be open-source friendly and modular.

---

## 📄 License

Choose any OSI-approved license you prefer (MIT recommended).

---

## ❤️ About

TogetherLog is designed to help people preserve moments that matter — elegantly, privately, and beautifully.


