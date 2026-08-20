# Portfolio Website

A fast, responsive personal portfolio built with **SvelteKit**, **Svelte 5**, **TypeScript**, and **Vite**.

---

## 🛠️ Tech Stack

* **Framework:** [SvelteKit](https://svelte.dev/docs/kit/introduction) (Svelte 5)
* **Language:** TypeScript
* **Build Tool:** [Vite](https://vite.dev/)
* **Plugins:** `@poppanator/sveltekit-svg` (SVG loading)
* **Adapter:** `@sveltejs/adapter-static` / `@sveltejs/adapter-auto`

---

## 🚀 Getting Started

### Prerequisites

Ensure you have [Node.js](https://nodejs.org/) installed on your machine.

### Installation

1. Clone the repository:
    ```bash
    git clone [https://github.com/your-username/portfolio.git](https://github.com/your-username/portfolio.git)
    cd portfolio
    ```

2. Install dependencies:
    ```bash
    npm install
    ```

---

## 💻 Development Commands

| Command | Description |
| --- | --- |
| `npm run dev` | Starts the local development server |
| `npm run build` | Builds the production-ready static site |
| `npm run preview` | Previews the production build locally |
| `npm run check` | Runs type checks via `svelte-check` |
| `npm run check:watch` | Runs type checks in watch mode |

---

## 📦 Deployment

This project uses `@sveltejs/adapter-static` to output static HTML/CSS/JS files:

1. Generate the static site build:
```bash
npm run build
```


2. Deploy the generated output in the `build/` folder to your static hosting platform (e.g., GitHub Pages, Vercel, Netlify, or Cloudflare Pages).