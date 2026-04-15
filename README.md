# Codex Business Studio Figma Mock

Vue 3 + Vite implementation of the Figma sidebar menu from `BS-Mockup-14.3`.

## Requirements

- Node.js 18+ recommended
- npm

## Install dependencies

```bash
npm install
```

## Start the development server

```bash
npm run dev
```

Vite will print a local URL in the terminal, usually:

```text
http://localhost:5173/
```

Open that URL in your browser to review the implementation.

## Build for production

```bash
npm run build
```

## Deploy to GitHub Pages

GitHub Actions deploys the app to GitHub Pages automatically on every push to `main`.

After pushing these files, make sure the repository Pages setting uses `GitHub Actions` as the source.

## Project structure

- `src/App.vue`: sidebar implementation
- `src/components/MenuIcon.vue`: inline SVG icon set
- `src/style.css`: layout and visual styling
