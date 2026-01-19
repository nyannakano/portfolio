# Terminal Portfolio

Personal portfolio website of **Gabriel Tsutomo Nakano** - Full Stack Developer.

A high-fidelity, single-page developer portfolio built as a pixel-perfect Linux Terminal (ZSH/Oh-My-Zsh style) interface.

## Preview

The entire UI is a terminal window centered on the screen, featuring:

- **Boot Sequence** - Fast-scrolling Linux-style system boot log
- **ASCII Art** - Large "NAKANO" logo displayed after boot
- **Interactive Terminal** - Clickable CLI navigation
- **Neofetch Sidebar** - System info display (OS, Shell, Uptime, etc.)
- **CRT Effects** - Scanlines and subtle flicker for authenticity

## Tech Stack

- **Vue 3** - Composition API with TypeScript
- **Tailwind CSS 4** - Utility-first styling
- **Vite** - Build tool and dev server
- **JetBrains Mono** - Monospace typography

## Features

| Command | Description |
|---------|-------------|
| `whoami` | Professional bio and description |
| `ls projects` | Grid of project cards with tech stacks |
| `neofetch` | System information display |
| `contact` | Social links and contact info |
| `clear` | Clear terminal output |
| `home` | Return to welcome screen |

### Visual Features

- Dark obsidian/slate terminal theme
- Traffic light window controls (red, yellow, green)
- Typewriter effect for all text output
- Blinking block cursor
- Smooth transitions and animations

## Project Setup

```sh
npm install
```

### Development

```sh
npm run dev
```

### Production Build

```sh
npm run build
```

### Type-Check

```sh
npm run type-check
```

### Lint

```sh
npm run lint
```
