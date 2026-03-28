# El Huey Coyote - Project Guide

## What is this?
Landing page for **El Huey Coyote**, a cumbia/music band. Single-page site with Mexican-inspired design (red, yellow, green palette). Hosted on Hostinger via FTP auto-deploy.

## Tech Stack
- Pure HTML/CSS/JS — no frameworks, no build step
- Single file: `index.html` (~700 lines, inline styles + scripts)
- Images in `assets/img/`
- Google Fonts: Bebas Neue, Permanent Marker, Oswald, Special Elite

## Architecture
- `index.html` — entire site (HTML + `<style>` + `<script>` inline)
- `assets/img/` — images (cortina, changarro, shows, merch, colabs, contacto, about)
- `.github/workflows/deploy.yml` — FTP deploy to Hostinger on push to `main`

## Deployment
- Auto-deploys on push to `main` via GitHub Actions → FTP to Hostinger `/public_html/`
- FTP credentials stored as GitHub Secrets: `FTP_HOST`, `FTP_USERNAME`, `FTP_PASSWORD`
- No build step needed — files deploy as-is

## Design
- CSS variables defined in `:root` (--rojo, --amarillo, --verde, --negro, --blanco, --naranja)
- Splash screen ("pantalla-inicio") with curtain animation
- Sections: shows, merch, colabs, contacto, about
- Social links bar at bottom
- Language: Spanish (lang="es")

## Conventions
- Commit messages use 🐺 emoji prefix
- All UI text in Spanish
- Mexican cultural aesthetic throughout
