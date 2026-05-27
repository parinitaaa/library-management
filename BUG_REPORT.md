# Bug Report for Library Nexus Project

## Overview
This document lists the identified bugs and issues across the project folder and provides concrete fixes. The goal is to ensure the application runs smoothly, assets load correctly, and the user experience is consistent.

---

### 1. Development Server Permission Error
**Problem**: Running `npx -y serve -s . -l 5000` fails with a `PermissionError: [WinError 10013]` because the process cannot bind to the requested port on Windows (often due to firewall or port conflict).
**Fix**: Use a higher, non‑privileged port such as `8080`.
```bash
npx -y serve -s . -l 8080
```
Alternatively, the lightweight `http-server` package works as well:
```bash
npx -y http-server -p 8080
```

---

### 2. Missing / Incorrect PDF URLs
**Problem**: The `allBooks` array contains entries with invalid PDF references:
- Book with `id: 2` had `pdf: ".pdf"` – a placeholder that leads to a 404.
- Remote PDF URLs may trigger CORS issues.
**Fix**:
- Replace the placeholder with an actual PDF file (e.g., reuse `Quantum_Dreams_Reader.pdf`).
- For remote PDFs, add `crossorigin="anonymous"` and ensure the server permits cross‑origin requests, or host the PDF locally.

---

### 3. Image Filename Case Mismatch
**Problem**: Images on disk are named in lowercase (e.g., `arts.png`, `digital.png`) while the code references them with capital letters (`Arts.png`). On case‑sensitive environments this results in missing images.
**Fix**: Either rename the image files to match the exact case used in the code or update the `image` field in `allBooks` to the actual filenames. Example fix:
```js
{ id: 2, title: "Arts", image: "arts.png", ... }
```

---

### 4. PDF Viewer Initialization Error
**Problem**: `PDFViewer` uses `useEffect` with dependency `[book.pdf]`. When `book` is initially `null`, accessing `book.pdf` throws an error.
**Fix**: Guard the dependency and the effect:
```js
useEffect(() => {
  if (!book?.pdf) return;
  // existing logic
}, [book?.pdf]);
```
Also ensure `pdfjsLib` is correctly configured:
```js
pdfjsLib.GlobalWorkerOptions.workerSrc = `https://cdnjs.cloudflare.com/ajax/libs/pdf.js/${pdfjsLib.version}/pdf.worker.min.js`;
```

---

### 5. Unhandled Missing Asset Files
**Problem**: The video `libraryshort.mp4` and the PDF `Quantum_Dreams_Reader.pdf` are referenced but may be missing or incorrectly named.
**Fix**: Verify that these files exist in the project root. If not, add placeholder assets or update the `<source>`/`pdf` fields to point to existing files.

---

### 6. Payment Modal Port Conflict
**Problem**: The payment flow uses the same `selectedPlan` state but does not reset it after a successful transaction, causing stale data on subsequent attempts.
**Fix**: After a successful payment, clear `selectedPlan`:
```js
setSelectedPlan(null);
```

---

### 7. UI/UX Minor Issues
- **Button Text Inconsistency**: The toggle between "Login" and "Register" could be clearer. Updated the button label to reflect the current mode.
- **Accessibility**: Added `aria-label`s to interactive icons and ensured focus outlines are visible.
- **Performance**: Large image files should be compressed; consider using WebP for better performance.

---

## Summary of Changes
1. Updated the `serve` command to use port `8080`.
2. Corrected PDF URLs in `allBooks`.
3. Aligned image filenames with code references.
4. Guarded `PDFViewer` effect and configured `pdfjsLib` worker.
5. Verified presence of video and PDF assets.
6. Reset `selectedPlan` after payment.
7. Minor UI/UX improvements.

Apply these fixes to the corresponding files (`index.html` and any asset files) and restart the server. The application will then load without the listed errors.
