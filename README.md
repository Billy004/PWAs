# 📱 Simple Progressive Web App (PWA)

This is a lightweight and minimal example of a Progressive Web App (PWA) built with plain HTML, CSS, and JavaScript. It's installable, works offline, and follows the core principles of modern web apps.

---

## 🚀 Features

- ✅ Installable on desktop and mobile
- ✅ Offline support using Service Worker
- ✅ Responsive design
- ✅ Manifest file with app metadata
- ✅ Works on all modern browsers

---

## 🛠️ How It Works

### Key Files

| File | Description |
|------|-------------|
| `index.html` | Main HTML page |
| `manifest.json` | App metadata for installability |
| `service-worker.js` | Caches assets for offline access |
| `icon.png` | App icon |
| `screenshot.png` | Screenshot used for richer install UI |

---

## 📦 Installation / Run Locally

To test this PWA locally, you’ll need a local server (due to Service Worker restrictions).

### Using VSCode Live Server:
1. Open project folder in VS Code
2. Right-click `index.html` and select **"Open with Live Server"**
3. Open the browser and click the install icon in the address bar

### Or use Python:
```bash
# Python 3
python -m http.server 8080
