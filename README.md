# Simple Progressive Web App (PWA) 📱

Welcome to the **Simple Progressive Web App (PWA)** project! This is a lightweight, minimal, and fully functional PWA built using vanilla **HTML**, **CSS**, and **JavaScript**. It demonstrates core PWA features such as offline support, installability, and responsive design, serving as an excellent starting point for learning about PWAs or building more complex applications.

---

## 🚀 Features

- **Installable**: Add to the home screen on mobile or desktop, with a native app-like experience.
- **Offline Support**: Uses a Service Worker to cache assets for offline access.
- **Responsive Design**: Adapts to various screen sizes, from phones to desktops.
- **Web App Manifest**: Includes `manifest.json` for app metadata and install prompts.
- **Cross-Browser Compatible**: Works on modern browsers (Chrome, Firefox, Safari, Edge).
- **Lightweight**: Minimal codebase with no external dependencies.

---

## 🛠️ How It Works

The PWA leverages modern web technologies to deliver a seamless user experience. Below are the core components:

### Key Files

| File | Description |
| --- | --- |
| `index.html` | Main HTML file containing the app’s structure and UI. |
| `manifest.json` | Defines app metadata (name, icons, theme) for installability. |
| `service-worker.js` | Caches assets for offline access and manages network requests. |
| `styles.css` | (Optional) CSS for responsive design and UI styling. |
| `icon.png` | App icon for home screen and install prompts. |
| `screenshot.png` | (Optional) Screenshot for richer install UI. |

### Core PWA Concepts

1. **Service Worker**:

   - Runs in the background to cache static assets during the `install` event.
   - Intercepts network requests to serve cached content during the `fetch` event.
   - Registered via `navigator.serviceWorker.register('/service-worker.js')`.

2. **Web App Manifest**:

   - A JSON file defining app metadata (e.g., name, icons, display mode).
   - Linked in `index.html` with `<link rel="manifest" href="/manifest.json">`.

3. **Responsive Design**:

   - Uses CSS media queries and flexible layouts for cross-device compatibility.
   - Includes `<meta name="viewport">` for proper mobile scaling.

4. **HTTPS Requirement**:

   - PWAs require a secure context (HTTPS) for Service Workers.
   - Use `localhost` or tools like `ngrok` for local development.

---

## 📦 Getting Started

Follow these steps to set up and run the PWA locally. Due to Service Worker restrictions, you must serve the app over HTTP/HTTPS (not `file://`).

### Prerequisites

- A modern web browser (Chrome, Firefox, Edge, Safari).
- A code editor (e.g., Visual Studio Code).
- A local web server (e.g., VS Code Live Server, Node.js `http-server`, or Python’s `http.server`).

### Installation

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/your-username/simple-pwa.git
   cd simple-pwa
   ```

2. **Choose a Local Server**:

   #### Option 1: VS Code Live Server

   - Install the **Live Server** extension in VS Code.
   - Open the project folder in VS Code.
   - Right-click `index.html` and select **"Open with Live Server"** (default: `http://localhost:5500`).

   #### Option 2: Python HTTP Server

   - Navigate to the project directory:

     ```bash
     cd simple-pwa
     ```

   - Start the server:

     ```bash
     python3 -m http.server 8000
     ```

   - Open `http://localhost:8000` in a browser.

   #### Option 3: Node.js `http-server`

   - Install `http-server` globally:

     ```bash
     npm install -g http-server
     ```

   - Start the server:

     ```bash
     cd simple-pwa
     http-server -p 8080
     ```

   - Open `http://localhost:8080`.

3. **Test the PWA**:

   - Open the URL in a browser.
   - Check Service Worker registration in DevTools (Application &gt; Service Workers).
   - Look for an **Install** icon in the address bar (e.g., `+` or download icon in Chrome).
   - Test offline mode by disconnecting from the internet and reloading the page.

### Troubleshooting

- **Service Worker Not Registering**:
  - Ensure the app is served over `localhost` or HTTPS.
  - Verify the path to `service-worker.js` in the registration script.
- **Install Prompt Not Appearing**:
  - Check `manifest.json` for valid JSON and required fields (`name`, `short_name`, `icons`, `start_url`).
  - Ensure the manifest is linked in `index.html`.
- **Offline Mode Not Working**:
  - Confirm `service-worker.js` caches the correct assets.
  - Inspect cached files in DevTools (Application &gt; Cache Storage).

---

## 🐳 Docker Setup

Containerize the PWA using **Docker** and **Nginx** for consistent deployment. This section guides you through building and running the app in a Docker container.

### Prerequisites

- Docker installed (Docker Desktop for Windows/Mac or Docker for Linux).

### Project Directory

Ensure the following files are in the project root:

```
simple-pwa/
├── index.html
├── manifest.json
├── service-worker.js
├── styles.css
├── icon.png
├── screenshot.png
├── Dockerfile
└── nginx.conf
```

### Step 1: Create `Dockerfile`

The `Dockerfile` configures the container to serve the PWA with Nginx.

```dockerfile
# Use the official Nginx image
FROM nginx:alpine

# Copy PWA files to Nginx's public directory
COPY . /usr/share/nginx/html

# Copy custom Nginx configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
```

### Step 2: Create `nginx.conf`

This file optimizes Nginx for serving the PWA.

```nginx
server {
    listen 80;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.html;

    # Serve static files, fallback to index.html for SPA
    location / {
        try_files $uri $uri/ /index.html;
    }

    # Cache static assets for performance
    location ~* \.(png|jpg|jpeg|gif|ico|css|js)$ {
        expires 1y;
        add_header Cache-Control "public";
    }

    # Enable gzip compression
    gzip on;
    gzip_types text/plain text/css application/json application/javascript;
}
```

### Step 3: Build the Docker Image

1. Navigate to the project directory:

   ```bash
   cd simple-pwa
   ```

2. Build the image:

   ```bash
   docker build -t simple-pwa .
   ```

### Step 4: Run the Container

Run the container, mapping port `8080` (host) to port `80` (container):

```bash
docker run -d -p 8080:80 --name pwa-container simple-pwa
```

### Step 5: Access the PWA

- Open `http://localhost:8080` in a browser.
- Test installability and offline functionality as described above.

### Step 6: Stop and Remove (Optional)

```bash
docker stop pwa-container
docker rm pwa-container
```

### Troubleshooting

- **Port Conflict**: Use a different host port (e.g., `-p 8081:80`).
- **Build Fails**: Check `Dockerfile` and `nginx.conf` for errors; ensure all files are present.
- **App Not Loading**: View logs with `docker logs pwa-container`.

---

## 🔧 Best Practices

1. **Optimize Assets**:

   - Compress images with tools like TinyPNG.
   - Minify CSS/JS with UglifyJS or cssnano.

2. **Cross-Browser Testing**:

   - Test on Safari, which has stricter Service Worker requirements.
   - Use BrowserStack for multi-device testing.

3. **Performance Monitoring**:

   - Run Lighthouse in Chrome DevTools to audit PWA quality.
   - Address caching, responsiveness, and manifest issues for a high score.

4. **Secure Deployment**:

   - Host on HTTPS-enabled platforms like Netlify, Vercel, or AWS.
   - Use a CDN for faster asset delivery.

5. **Service Worker Updates**:

   - Implement `skipWaiting` and `clients.claim` in `service-worker.js` for seamless cache updates.

---

## 📚 Resources

- MDN: Progressive Web Apps
- Google PWA Checklist
- Service Worker Cookbook
- Docker Documentation
- Nginx Beginner’s Guide

---

## ❓ FAQs

**Q: Why doesn’t offline mode work locally?**\
A: Serve the app over `localhost` or HTTPS, as Service Workers don’t work with `file://` URLs. Check `service-worker.js` for correct caching.

**Q: How do I customize the PWA?**\
A: Modify `index.html` for content, `styles.css` for design, and `manifest.json` for metadata (e.g., name, colors).

**Q: Can I deploy this to production?**\
A: Yes, use HTTPS-enabled hosting (Netlify, Vercel, AWS). Optimize assets and test thoroughly.

**Q: How do I add dynamic content?**\
A: Use JavaScript to fetch API data in `index.html`. Extend `service-worker.js` with Workbox for advanced caching.

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/YourFeature`).
3. Commit changes (`git commit -m "Add YourFeature"`).
4. Push to the branch (`git push origin feature/YourFeature`).
5. Open a Pull Request.

Please include tests and update documentation as needed.

---

## 📄 License

This project is licensed under the MIT License.

---

Built with ❤️ by \[Your Name/Organization\]. Happy coding!