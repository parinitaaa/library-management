# Run Instructions

## Prerequisites
- Node.js (v18 or newer) installed.
- A working internet connection (CDN assets are loaded from the web).

## Steps
1. **Open a terminal** in the project root (`mern1`).
2. **Install the `serve` package** (once) if not already available:
   ```bash
   npm -y install serve
   ```
3. **Start the static server** on a non‑privileged port (e.g., 8080) to avoid Windows permission errors:
   ```bash
   npx -y serve -s . -l 8080
   ```
4. **Open your browser** and navigate to:
   ```
   http://localhost:8080
   ```
5. The application should load with the updated bug fixes.

## Alternative
If you prefer Python's simple server, you can also run:
```bash
python -m http.server 8080
```
and then visit `http://localhost:8080`.
