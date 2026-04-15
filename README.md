# nextjs-starter

A modern, opinionated Next.js starter project builder using TypeScript, Tailwind CSS v4, ESLint, App Router, Turbopack, shadcn/ui, Lucide icons, and dark mode, with example templates ready to clone and build on.

This document is a **build guide**: follow the steps to produce a project that matches [What's Included](#whats-included). If you clone this repository and the root folder does not yet contain a generated Next.js app (no `package.json`), start from [Step 1](#step-1--scaffold-the-nextjs-app)—including scaffolding into `.`—before running `pnpm install` on its own.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [How To Build This Template](#how-to-build-this-template)
  - [Step 1 — Scaffold the Next.js App](#step-1--scaffold-the-nextjs-app)
  - [Step 2 — Verify the Project Structure](#step-2--verify-the-project-structure)
  - [Step 3 — Add shadcn/ui](#step-3--add-shadcnui)
  - [Step 4 — Add Lucide Icons](#step-4--add-lucide-icons)
  - [Step 5 — Add Dark Mode with next-themes](#step-5--add-dark-mode-with-next-themes)
  - [Step 6 — Wire Up the Theme Provider](#step-6--wire-up-the-theme-provider)
  - [Step 7 — Add a Theme Toggle Component](#step-7--add-a-theme-toggle-component)
- [Using This Template for a New Project](#using-this-template-for-a-new-project)
- [Optional Additions](#optional-additions)
  - [Add More shadcn Components](#add-more-shadcn-components)
  - [Customize Tailwind Tokens](#customize-tailwind-tokens)
  - [Customize shadcn Theme Colors](#customize-shadcn-theme-colors)
  - [Add Fonts](#add-fonts)
- [What's Included](#whats-included)
- [Scripts](#scripts)

---

## Prerequisites

Make sure the following are installed on your machine before anything else.

- **Node.js 20.9+** — check with `node -v`
- **pnpm** — install with `npm i -g pnpm` or check with `pnpm -v`
- A code editor (VS Code recommended)

---

## How To Build This Template

Follow these steps exactly to reproduce this stack from scratch, or use them as a checklist against a project that already contains the generated files.

---

### Step 1 — Scaffold the Next.js App

Navigate to where you want the project, and run `create-next-app` with `--yes` to accept all recommended defaults. That enables TypeScript, Tailwind CSS v4, ESLint, App Router, Turbopack, and the `@/*` import alias, and includes `AGENTS.md` / `CLAUDE.md` to guide AI coding agents.

**New directory** (recommended if the folder should be empty):

```bash
pnpm create next-app@latest my-app --yes
cd my-app
```

**Current directory** (e.g. after `git clone …` into this repo’s root): `create-next-app` refuses to run if the folder already has conflicting files (such as this `README.md`). Temporarily move or rename those files, scaffold, then merge any notes you care to keep back into the project README.

```bash
mv README.md README.build-guide.md   # example; resolve any other listed conflicts the CLI prints
pnpm create next-app@latest . --yes
# optional: fold README.build-guide.md into the new README.md from create-next-app
```

With npm: `npx create-next-app@latest . --yes` (same flags and conflict behavior). The generated `package.json` **name** must be a valid npm package name; if the CLI complains about the folder name (for example uppercase letters), rename the directory or set the name when prompted.

> **What `--yes` sets up for you:**
> - TypeScript
> - Tailwind CSS v4 (configured via `globals.css`, no `tailwind.config.ts` needed)
> - ESLint
> - App Router (`app/` directory)
> - Turbopack (default dev bundler)
> - `@/*` import alias pointing to the project root
> - `AGENTS.md` + `CLAUDE.md` for AI coding agent guidance

If you prefer to answer prompts interactively (e.g. to enable a `src/` directory or choose a linter), run without `--yes`:

```bash
pnpm create next-app@latest my-app
```

You'll be asked:

```
What is your project named? my-app
Would you like to use the recommended Next.js defaults?
  > Yes, use recommended defaults - TypeScript, ESLint, Tailwind CSS, App Router, AGENTS.md
    No, reuse previous settings
    No, customize settings
```

Choosing **"No, customize settings"** gives you fine-grained control:

```
Would you like to use TypeScript?               No / Yes
Which linter would you like to use?             ESLint / Biome / None
Would you like to use React Compiler?           No / Yes
Would you like to use Tailwind CSS?             No / Yes
Would you like your code inside a src/ dir?    No / Yes
Would you like to use App Router? (recommended) No / Yes
Would you like to customize the import alias?   No / Yes
What import alias would you like configured?    @/*
Would you like to include AGENTS.md?            No / Yes
```

Verify the dev server starts:

```bash
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000). Stop the server with `Ctrl+C` when done.

---

### Step 2 — Verify the Project Structure

The scaffold produces this layout:

```
my-app/
├── app/
│   ├── favicon.ico
│   ├── globals.css       # Tailwind v4 config lives here (no tailwind.config.ts)
│   ├── layout.tsx        # Root layout — html + body tags
│   └── page.tsx          # Home page
├── public/               # Static assets
├── AGENTS.md             # AI agent guidance (references CLAUDE.md)
├── CLAUDE.md             # Claude-specific agent guidance
├── eslint.config.mjs
├── next.config.ts
├── package.json
├── postcss.config.mjs    # Tailwind v4 + PostCSS (default app-tw template)
└── tsconfig.json
```

> **Tailwind v4 note:** Configuration is done entirely inside `globals.css` using `@import "tailwindcss"` and CSS custom properties — no `tailwind.config.ts` file is generated by default.

---

### Step 3 — Add shadcn/ui

shadcn/ui is a collection of copy-owned components built on Radix UI primitives and styled with Tailwind CSS. Components are added directly into your codebase, not as an opaque npm dependency.

> **Note:** The shadcn CLI targets Tailwind v4. If you are on Tailwind v3, use `shadcn@2.3.0` instead.

Run the initializer:

```bash
pnpm dlx shadcn@latest init
```

The CLI will ask a few questions:

```
Which style would you like to use? › Default
Which color would you like to use as the base color? › Neutral
```

This creates a `components.json` config file, adds CSS variable definitions to `globals.css`, and sets up `lib/utils.ts` with the `cn()` helper.

Add your first component to confirm it's working:

```bash
pnpm dlx shadcn@latest add button
```

The `Button` component is now available at `components/ui/button.tsx`. Import and use it:

```tsx
import { Button } from "@/components/ui/button"

export default function Page() {
  return <Button>Click me</Button>
}
```

---

### Step 4 — Add Lucide Icons

Lucide provides a clean, consistent icon set with first-class React support.

```bash
pnpm add lucide-react
```

Usage:

```tsx
import { Sun, Moon, Menu } from "lucide-react"

export function MyComponent() {
  return <Sun className="h-5 w-5" />
}
```

Browse the full icon catalogue at [lucide.dev](https://lucide.dev).

---

### Step 5 — Add Dark Mode with next-themes

`next-themes` handles system preference detection, persisting the user's choice, and switching themes without flash.

```bash
pnpm add next-themes
```

Create a `ThemeProvider` wrapper at `components/theme-provider.tsx`:

```tsx
"use client"

import { ThemeProvider as NextThemesProvider } from "next-themes"
import type { ThemeProviderProps } from "next-themes"

export function ThemeProvider({ children, ...props }: ThemeProviderProps) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>
}
```

---

### Step 6 — Wire Up the Theme Provider

Open `app/layout.tsx` and wrap the app in the `ThemeProvider`. The `suppressHydrationWarning` prop on `<html>` prevents React from complaining about the server/client theme mismatch on first render.

```tsx
import type { Metadata } from "next"
import { Geist, Geist_Mono } from "next/font/google"
import { ThemeProvider } from "@/components/theme-provider"
import "./globals.css"

const geistSans = Geist({ variable: "--font-geist-sans", subsets: ["latin"] })
const geistMono = Geist_Mono({ variable: "--font-geist-mono", subsets: ["latin"] })

export const metadata: Metadata = {
  title: "My App",
  description: "My Next.js app",
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" suppressHydrationWarning>
      <body className={`${geistSans.variable} ${geistMono.variable} antialiased`}>
        <ThemeProvider
          attribute="class"
          defaultTheme="system"
          enableSystem
          disableTransitionOnChange
        >
          {children}
        </ThemeProvider>
      </body>
    </html>
  )
}
```

Key `ThemeProvider` props:

| Prop | Effect |
|---|---|
| `attribute="class"` | Adds `dark` class to `<html>` — works with Tailwind's `dark:` prefix |
| `defaultTheme="system"` | Follows OS preference on first visit |
| `enableSystem` | Enables the `"system"` option |
| `disableTransitionOnChange` | Prevents jarring CSS transitions during theme switch |

---

### Step 7 — Add a Theme Toggle Component

Add the `ModeToggle` using the shadcn `DropdownMenu` component and Lucide icons:

```bash
pnpm dlx shadcn@latest add dropdown-menu
```

Create `components/mode-toggle.tsx`:

```tsx
"use client"

import { Moon, Sun } from "lucide-react"
import { useTheme } from "next-themes"
import { Button } from "@/components/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"

export function ModeToggle() {
  const { setTheme } = useTheme()

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="outline" size="icon">
          <Sun className="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
          <Moon className="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
          <span className="sr-only">Toggle theme</span>
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="end">
        <DropdownMenuItem onClick={() => setTheme("light")}>Light</DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("dark")}>Dark</DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("system")}>System</DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  )
}
```

Drop `<ModeToggle />` wherever you want the toggle to appear (e.g. in a navbar or on the home page).

---

## Using This Template for a New Project

To start a new project from this repository:

1. **Clone the repo** (pick your own folder name):

   ```bash
   git clone git@github.com:adybose/nextjs-starter.git my-new-project
   cd my-new-project
   ```

2. **Generate the Next.js app in the clone** if there is not already a `package.json` in the root. Resolve file conflicts the CLI reports (often by temporarily moving this guide’s `README.md`), then run:

   ```bash
   pnpm create next-app@latest . --yes
   ```

   Continue with **Step 3** onward in [How To Build This Template](#how-to-build-this-template) (shadcn, Lucide, dark mode, theme toggle). If the clone already contains a full generated app, skip scaffolding and only run `pnpm install` when you add new dependencies.

3. **Optional — reset git history** so the new project does not keep this template’s commits:

   ```bash
   rm -rf .git
   git init
   git add .
   git commit -m "Initial commit from template"
   ```

4. **Update project metadata** in `app/layout.tsx` (title, description) and `package.json` (name, version).

5. **Start the dev server:**

   ```bash
   pnpm dev
   ```

---

## Optional Additions

The sections below are **not** part of the minimal stack you get from [What's Included](#whats-included) after Steps 1–7. They are common next steps on a real app:

- **Extra shadcn/ui components** beyond what you add in Steps 3 and 7 (for example card, dialog, more form controls).
- **Tailwind design tokens** (`@theme` in `globals.css`).
- **shadcn theme colors** (`:root` / `.dark` CSS variables).
- **Custom fonts** via `next/font` and `@theme` in CSS.

---

### Add More shadcn Components

Browse available components at [ui.shadcn.com/docs/components](https://ui.shadcn.com/docs/components) and add them as needed:

```bash
# Examples
pnpm dlx shadcn@latest add card
pnpm dlx shadcn@latest add dialog
pnpm dlx shadcn@latest add input
pnpm dlx shadcn@latest add select
pnpm dlx shadcn@latest add table
pnpm dlx shadcn@latest add sonner
pnpm dlx shadcn@latest add sidebar
```

> **Note:** The older Radix **toast** component is deprecated in shadcn/ui in favor of **Sonner** (`add sonner`). See [Sonner](https://ui.shadcn.com/docs/components/sonner).

Components land in `components/ui/` as plain TypeScript files you can read and modify freely.

---

### Customize Tailwind Tokens

With Tailwind v4, all customization lives in `app/globals.css`. Add custom tokens under `@theme`:

```css
@import "tailwindcss";

@theme {
  --color-brand: oklch(0.6 0.2 250);
  --font-display: "Inter", sans-serif;
  --radius-card: 1rem;
}
```

These become Tailwind utilities automatically (e.g. `bg-brand`, `font-display`, `rounded-card`).

---

### Customize shadcn Theme Colors

shadcn uses CSS custom properties for its color system. Edit the `:root` and `.dark` blocks that `shadcn init` added to your `globals.css`:

```css
:root {
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
  --primary: oklch(0.205 0 0);
  --primary-foreground: oklch(0.985 0 0);
  /* ... */
}

.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
  /* ... */
}
```

Use the visual theme builder at [ui.shadcn.com/themes](https://ui.shadcn.com/themes) to generate your palette and paste it directly into `globals.css`.

---

### Add Fonts

Next.js has a built-in `next/font` module that self-hosts Google Fonts with zero layout shift. Edit `app/layout.tsx`:

```tsx
import { Inter } from "next/font/google"

const inter = Inter({ subsets: ["latin"], variable: "--font-inter" })

// In RootLayout:
<body className={`${inter.variable} antialiased`}>
```

Then reference it in `globals.css`:

```css
@theme {
  --font-sans: var(--font-inter), sans-serif;
}
```

---

## What's Included

After you complete the scaffold (Step 1) and the shadcn / Lucide / theme steps (Steps 3–7), the project includes:

| Feature | Details |
|---|---|
| **Framework** | Next.js (latest) with App Router |
| **Language** | TypeScript (strict mode) |
| **Styling** | Tailwind CSS v4 (CSS-first config in `globals.css`) |
| **Components** | shadcn/ui (Radix UI + Tailwind, copy-owned) |
| **Icons** | Lucide React |
| **Dark Mode** | `next-themes` with system preference detection |
| **Linting** | ESLint with Next.js recommended config |
| **Bundler** | Turbopack (dev), Webpack-compatible (build) |
| **Import Alias** | `@/*` → project root |
| **AI Agent Guidance** | `AGENTS.md` + `CLAUDE.md` |

---

## Scripts

```bash
pnpm dev        # Start dev server at http://localhost:3000 (Turbopack)
pnpm build      # Production build
pnpm start      # Start production server (requires pnpm build first)
pnpm lint       # Run ESLint
```

To apply ESLint fixes in one shot, run `pnpm exec eslint . --fix`, or add a `"lint:fix": "eslint . --fix"` script to `package.json` (the default `create-next-app` template only defines `lint`).