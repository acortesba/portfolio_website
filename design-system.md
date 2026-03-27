# Design System

A reference for the visual language used across the portfolio. Consult this when updating styles or building new components to keep things consistent.

## Design Philosophy

The aesthetic pulls from two directions: 80s/90s computer interfaces (BIOS screens, terminal UIs, pixel fonts) and contemporary web trends (glassmorphism, smooth transitions, generous whitespace). The goal is nostalgic without feeling dated—a CRT glow paired with modern readability.

Everything should feel functional. Decorative elements earn their place by serving a purpose, whether that's guiding attention or reinforcing the theme. Accessibility stays at WCAG AA minimum. Animations run butter-smooth or they don't run at all.

## Color Palette

### Six Theme System

The site offers six visual themes, each with its own personality and use case. All themes use CSS custom properties for instant switching with zero flash.

#### 1. Day (Default)
Clean, professional light theme. Perfect for daytime browsing or corporate settings.

```css
:root {
  --primary-text: #333333;
  --secondary-text: #666666;
  --accent-primary: #667eea;
  --accent-secondary: #764ba2;
  --background-primary: #ffffff;
  --background-secondary: #f8f9fa;
  --border-color: #e9ecef;
  --card-background: rgba(255, 255, 255, 0.95);
  --shadow-color: rgba(0, 0, 0, 0.1);
}
```

**Use case:** Professional presentations, job applications, daytime browsing

#### 2. Night
Dark mode without harsh blacks. Easy on the eyes for nighttime reading.

```css
body.night-mode {
  --primary-text: #ffffff;
  --secondary-text: #b0b0b0;
  --accent-primary: #4169E1;
  --accent-secondary: #1e3a8a;
  --background-primary: #1a1a1a;
  --background-secondary: #2d2d2d;
  --border-color: #404040;
  --card-background: rgba(45, 45, 45, 0.95);
  --shadow-color: rgba(0, 0, 0, 0.5);
}
```

**Use case:** Night reading, low-light environments, OLED displays

#### 3. Cyberpunk
Neon colors and high contrast. Hacker terminal aesthetic.

```css
body.cyberpunk-mode {
  --primary-text: #ff00ff;
  --secondary-text: #ff0080;
  --accent-primary: #00ffff;
  --accent-secondary: #0080ff;
  --background-primary: #0f0f23;
  --background-secondary: #1a0f2e;
  --border-color: rgba(255, 0, 255, 0.4);
  --card-background: rgba(26, 15, 46, 0.95);
  --shadow-color: rgba(255, 0, 255, 0.3);
}
```

**Use case:** Gaming, creative showcases, standing out

#### 4. Monochrome
High-fashion minimalism with beige/tan backgrounds and pure black text.

```css
body.monochrome-mode {
  --primary-text: #000000;
  --secondary-text: #404040;
  --accent-primary: #666666;
  --accent-secondary: #999999;
  --background-primary: #f5deb3; /* wheat */
  --background-secondary: #f0e68c; /* khaki */
  --border-color: #d2b48c; /* tan */
  --card-background: rgba(245, 222, 179, 0.95);
  --shadow-color: rgba(0, 0, 0, 0.15);
}
```

**Use case:** Art portfolios, minimalist aesthetics, print-friendly

#### 5. Blueprint
Technical drawing aesthetic with white-on-blue styling. Architect/engineer vibe.

```css
body.blueprint-mode {
  --primary-text: #ffffff;
  --secondary-text: #e0e0e0;
  --accent-primary: #3b82f6;
  --accent-secondary: #1e3a8a;
  --background-primary: #1e3a8a;
  --background-secondary: #2d4a7c;
  --border-color: #60a5fa;
  --card-background: rgba(45, 74, 124, 0.95);
  --shadow-color: rgba(59, 130, 246, 0.3);
}
```

**Use case:** Technical projects, architectural work, engineering portfolios

#### 6. Retro
Nostalgic vintage computing theme with warm amber/green tones.

```css
body.retro-mode {
  --primary-text: #00ff00;
  --secondary-text: #00cc00;
  --accent-primary: #ffaa00;
  --accent-secondary: #ff8800;
  --background-primary: #000000;
  --background-secondary: #1a1a00;
  --border-color: rgba(0, 255, 0, 0.3);
  --card-background: rgba(26, 26, 0, 0.95);
  --shadow-color: rgba(0, 255, 0, 0.2);
}
```

**Use case:** Retro gaming, nostalgia, vintage computing enthusiasts

### Using Colors Correctly

**Always use CSS variables, never hardcode colors:**

```css
/* ✅ Good */
.button {
  color: var(--primary-text);
  background: var(--accent-primary);
  border: 1px solid var(--border-color);
}

/* ❌ Bad */
.button {
  color: #333333;
  background: #667eea;
  border: 1px solid #e9ecef;
}
```

This ensures all components switch themes correctly with zero custom code per theme.

### Blend Modes for Effects

Light themes (Day, Monochrome) use `multiply` blend mode for subtle effects.
Dark themes (Night, Cyberpunk, Blueprint, Retro) use `screen` for vibrant effects.

```css
:root, body.monochrome-mode {
  --holo-blend-mode: multiply;
}

body.night-mode, 
body.cyberpunk-mode, 
body.blueprint-mode,
body.retro-mode {
  --holo-blend-mode: screen;
}

.holographic-effect {
  mix-blend-mode: var(--holo-blend-mode);
}
```

## Typography

### Fonts

**Primary: VT323**  
Monospace pixel font for headers, buttons, and retro elements. Gives the distinctive retro-computing aesthetic.

```css
font-family: 'VT323', monospace;
```

**Secondary: Press Start 2P**  
8-bit game font for special callouts (used sparingly to avoid readability issues).

```css
font-family: 'Press Start 2P', monospace;
```

**Body: Inter** (fallback)
Modern sans-serif for long-form blog content to improve readability.

```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
```

**Fallback: Courier New**  
System monospace if web fonts fail to load.

### Font Sizes

Mobile-first scaling using CSS variables:

```css
:root {
  --font-xs: 0.875rem;   /* 14px */
  --font-sm: 1rem;       /* 16px */
  --font-base: 1.125rem; /* 18px */
  --font-lg: 1.25rem;    /* 20px */
  --font-xl: 1.5rem;     /* 24px */
  --font-2xl: 2rem;      /* 32px */
  --font-3xl: 2.5rem;    /* 40px */
  --font-4xl: 3rem;      /* 48px */
}

@media (max-width: 768px) {
  :root {
    --font-xs: 0.75rem;   /* 12px */
    --font-sm: 0.875rem;  /* 14px */
    --font-base: 1rem;    /* 16px */
    --font-lg: 1.125rem;  /* 18px */
    --font-xl: 1.25rem;   /* 20px */
    --font-2xl: 1.5rem;   /* 24px */
    --font-3xl: 2rem;     /* 32px */
    --font-4xl: 2.5rem;   /* 40px */
  }
}
```

### Text Styles

**Headers:**
```css
h1 {
  font-family: 'VT323', monospace;
  font-size: var(--font-4xl);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--primary-text);
}

h2 {
  font-family: 'VT323', monospace;
  font-size: var(--font-3xl);
  letter-spacing: 0.08em;
  color: var(--primary-text);
}
```

**Body Text:**
```css
p {
  font-family: 'VT323', monospace;
  font-size: var(--font-base);
  line-height: 1.6;
  letter-spacing: 0.02em;
  color: var(--primary-text);
}

/* Blog content uses Inter for readability */
.blog-content p {
  font-family: 'Inter', sans-serif;
  line-height: 1.8;
}
```

**Code Blocks:**
```css
code {
  font-family: 'VT323', 'Courier New', monospace;
  background: var(--background-secondary);
  padding: 2px 6px;
  border-radius: 4px;
  font-size: 0.95em;
  color: var(--accent-primary);
}

pre code {
  display: block;
  padding: 15px;
  overflow-x: auto;
  border: 1px solid var(--border-color);
}
```

## Icon System

**Files:** `shared/icons/themeIcons.js`, `shared/icons/themeIcons.css`

A centralized SVG icon system that injects icons based on CSS class names.

### Usage

```html
<!-- Add icon class to any element -->
<span class="star-svg"></span>
<span class="lock-svg icon-lg"></span>
<span class="starGolden-svg icon-inline"></span>
```

### Golden Star Icon (Blueprint Theme Reward)

Special golden star icon for Theme Hopper completion:

```javascript
// In themeIcons.js
starGolden: `<svg viewBox="0 0 24 24">
  <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2" 
    fill="#FFD700" stroke="#FFA500" stroke-width="0.8"/>
</svg>`
```

```css
/* Golden star styling with glow animation */
.star-container .starGolden-svg,
.starGolden-svg.icon-inline {
  filter: drop-shadow(0 0 4px #FFD700) drop-shadow(0 0 8px #FFA500);
  animation: starGlow 2s ease-in-out infinite, starSpin 8s linear infinite;
}

@keyframes starGlow {
  0%, 100% { filter: drop-shadow(0 0 4px #FFD700) drop-shadow(0 0 8px #FFA500); }
  50% { filter: drop-shadow(0 0 8px #FFD700) drop-shadow(0 0 16px #FFA500); }
}

@keyframes starSpin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
```

### Icon Injection

Icons are automatically injected via MutationObserver when elements with matching class names are added to the DOM.

## Component Styles

### Buttons

Primary button with gradient and hover effect:

```css
.button-primary {
  font-family: 'VT323', monospace;
  font-size: 1.1rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  padding: 12px 24px;
  border: 2px solid var(--accent-primary);
  background: linear-gradient(135deg, 
    var(--accent-primary), 
    var(--accent-secondary)
  );
  color: #ffffff;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.button-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px var(--shadow-color);
}

.button-primary:active {
  transform: translateY(0);
}
```

Secondary button (outlined):

```css
.button-secondary {
  font-family: 'VT323', monospace;
  font-size: 1.1rem;
  text-transform: uppercase;
  padding: 10px 22px;
  border: 2px solid var(--accent-primary);
  background: transparent;
  color: var(--accent-primary);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.button-secondary:hover {
  background: var(--accent-primary);
  color: #ffffff;
}
```

### Cards

Standard card with glassmorphism effect:

```css
.card {
  background: var(--card-background);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 25px;
  border: 1px solid var(--border-color);
  box-shadow: 0 10px 30px var(--shadow-color);
  transition: all 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 40px var(--shadow-color);
}
```

Project card (3D folder container):

```css
.project-card {
  background: var(--background-secondary);
  border-radius: 12px;
  padding: 15px;
  border: 2px solid var(--border-color);
  transition: all 0.3s ease;
  position: relative;
  overflow: visible;
}

.project-card:hover {
  border-color: var(--accent-primary);
  box-shadow: 0 10px 30px var(--shadow-color);
}

.project-card:hover .folder-label {
  opacity: 1;
  transform: translateY(-10px);
}
```

### Theme Toggle

Fixed position toggle in top-right corner:

```css
.theme-selector {
  position: fixed;
  top: 20px;
  right: 80px;
  z-index: 1000;
  background: var(--background-secondary);
  border: 2px solid var(--border-color);
  border-radius: 12px;
  padding: 8px;
  backdrop-filter: blur(10px);
  display: flex;
  gap: 8px;
}

.theme-option {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 3px solid transparent;
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.theme-option:hover {
  transform: scale(1.1);
  border-color: var(--accent-primary);
}

.theme-option.active {
  border-color: var(--accent-primary);
  box-shadow: 0 0 20px var(--accent-primary);
}

/* Theme preview colors */
.theme-option[data-theme="day"] {
  background: linear-gradient(135deg, #667eea, #764ba2);
}

.theme-option[data-theme="night"] {
  background: linear-gradient(135deg, #1a1a1a, #4169E1);
}

.theme-option[data-theme="cyberpunk"] {
  background: linear-gradient(135deg, #ff00ff, #00ffff);
}

.theme-option[data-theme="monochrome"] {
  background: linear-gradient(135deg, #f5deb3, #d2b48c);
}

.theme-option[data-theme="blueprint"] {
  background: linear-gradient(135deg, #1e3a8a, #60a5fa);
}

.theme-option[data-theme="retro"] {
  background: linear-gradient(135deg, #000000, #00ff00);
}
```

### Profile Card (Holographic Effect) - Updated January 2026

**Files:** `portfolio/js/profileCard.js`, `portfolio/css/profileCard.css`

Collector's card aesthetic with mouse tracking and layered depth effects.

#### Structure

The profile card uses a layered approach:
- **Background layer** (z-index 0): Profile header text (name + title)
- **Image layer** (z-index 1): Photo as CSS background via `--avatar-url` variable
- **Gradient layer** (z-index 2): Holographic conic gradient effect
- **Content layer** (z-index 4): Footer with handle and Instagram link

#### Depth Effect on Hover

Text starts behind the image and emerges on hover:

```css
.profile-header {
  position: absolute;
  top: 32px;
  z-index: 0;
  transition: z-index 0s 0.3s, transform 0.4s ease;
}

.profile-card:hover .profile-header {
  z-index: 5;
  transform: translateY(-5px);
  transition: z-index 0s 0s, transform 0.4s ease;
}
```

#### Theme-Dependent Text Outlines

Text uses theme background color for readable outlines:

```css
.profile-name {
  text-shadow: 
    1px 1px 0 var(--bg-primary),
    -1px -1px 0 var(--bg-primary),
    1px -1px 0 var(--bg-primary),
    -1px 1px 0 var(--bg-primary);
}

.profile-card:hover .profile-name {
  text-shadow: 
    1px 1px 0 var(--bg-primary),
    -1px -1px 0 var(--bg-primary),
    1px -1px 0 var(--bg-primary),
    -1px 1px 0 var(--bg-primary),
    0 0 12px var(--glow-color);
}
```

#### Background Image Layer

Photo is set via CSS custom property for easy updates:

```css
.profile-card-bg {
  position: absolute;
  inset: 24px;
  background-image: var(--avatar-url);
  background-size: cover;
  background-position: calc(50% - 20px) top;
  z-index: 1;
}
```

#### Holographic Gradient Overlay

```css
.profile-card::after {
  background: conic-gradient(
    from calc(var(--pointer-from-left) * 360deg + 90deg) 
    at var(--pointer-x) var(--pointer-y), 
    var(--accent-primary) 0deg,
    var(--accent-secondary) 60deg,
    var(--accent-tertiary) 120deg,
    var(--accent-primary) 180deg
  );
  mix-blend-mode: var(--holo-blend-mode);
  z-index: 2;
}
```

#### JavaScript Component

```javascript
class ProfileCard {
  constructor(container, options = {}) {
    this.options = {
      name: options.name || 'ALBERTO C. B.',
      title: options.title || 'UX & APP DESIGNER',
      avatarUrl: options.avatarUrl || '/portfolio/components/profileCard/profile-photo.png',
      // ... other options
    };
  }
  
  render() {
    this.container.innerHTML = `
      <div class="profile-card" style="--avatar-url: url('${this.options.avatarUrl}')">
        <div class="profile-card-bg"></div>
        <div class="profile-header">...</div>
        <div class="profile-card-content">...</div>
      </div>
    `;
  }
}
```

## Layout Patterns

### Grid System

Responsive grid for project cards:

```css
.projects-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
  padding: 20px;
}

@media (max-width: 1200px) {
  .projects-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@media (max-width: 768px) {
  .projects-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
  }
}

@media (max-width: 480px) {
  .projects-grid {
    grid-template-columns: 1fr;
    gap: 10px;
  }
}
```

### 3D Folder Model Layout

Floating 3D models in 4x2 grid:

```css
.folder-canvas {
  width: 160px;
  height: 160px;
  border-radius: 10px;
  background: transparent;
  transition: transform 0.3s ease;
}

.folder-canvas:hover {
  transform: scale(1.05);
}

.folder-label {
  position: absolute;
  bottom: -30px;
  left: 50%;
  transform: translateX(-50%) translateY(10px);
  background: var(--background-secondary);
  color: var(--primary-text);
  padding: 5px 12px;
  border-radius: 8px;
  font-size: 0.9rem;
  opacity: 0;
  transition: all 0.3s ease;
  pointer-events: none;
  white-space: nowrap;
  border: 1px solid var(--border-color);
}
```

### Modal Viewer

Project modal (80% screen width for optimal viewing):

```css
.project-modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  max-width: 1400px;
  max-height: 90vh;
  background: var(--background-primary);
  border-radius: 20px;
  border: 2px solid var(--border-color);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
  overflow: hidden;
  z-index: 2000;
}

.project-modal-content {
  padding: 30px;
  overflow-y: auto;
  max-height: calc(90vh - 80px);
}

@media (max-width: 768px) {
  .project-modal {
    width: 95%;
    max-height: 95vh;
  }
  
  .project-modal-content {
    padding: 20px;
  }
}
```

### Flexbox for Headers

Header with logo and navigation:

```css
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 40px;
  background: var(--background-secondary);
  border-bottom: 2px solid var(--border-color);
}

.nav-links {
  display: flex;
  gap: 30px;
  align-items: center;
}

.nav-links a {
  color: var(--primary-text);
  text-decoration: none;
  font-size: 1.1rem;
  transition: color 0.3s ease;
}

.nav-links a:hover {
  color: var(--accent-primary);
}

@media (max-width: 768px) {
  .header {
    flex-direction: column;
    gap: 15px;
    padding: 15px 20px;
  }
  
  .nav-links {
    flex-direction: column;
    gap: 10px;
  }
}
```

## Animations

### Theme Transitions

Smooth color transitions when switching themes:

```css
* {
  transition: 
    background-color 0.3s ease,
    color 0.3s ease,
    border-color 0.3s ease;
}

/* Prevent transition on page load */
.preload * {
  transition: none !important;
}
```

Remove `.preload` class after page loads:

```javascript
window.addEventListener('load', () => {
  document.body.classList.remove('preload');
});
```

### Loading Animations

BIOS-style loading sequence with 3D exit animation:

```css
.loading-line {
  opacity: 0;
  animation: fadeInUp 0.5s ease-out forwards;
}

.loading-line:nth-child(1) { animation-delay: 0.2s; }
.loading-line:nth-child(2) { animation-delay: 0.4s; }
.loading-line:nth-child(3) { animation-delay: 0.6s; }
.loading-line:nth-child(4) { animation-delay: 0.8s; }

/* BIOS exit animation with 3D perspective */
@keyframes biosExit {
  0% { 
    opacity: 1; 
    transform: scale(1) perspective(1000px) rotateX(0deg); 
  }
  100% { 
    opacity: 0; 
    transform: scale(1.1) perspective(1000px) rotateX(-5deg); 
  }
}

.bios-overlay.exiting {
  animation: biosExit 0.6s ease-out forwards;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

### Hero Text Animation (Claude.com-Style)

Rotating slogans with RGB color cycling:

```css
.animated-text-container {
  font-family: 'VT323', monospace;
  font-size: 3rem;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  
  /* RGB color cycling (values set by JavaScript) */
  --rgb-r: 0.5;
  --rgb-g: 0.5;
  --rgb-b: 0.5;
  
  color: rgb(
    calc(var(--rgb-r) * 255),
    calc(var(--rgb-g) * 255),
    calc(var(--rgb-b) * 255)
  );
  
  transition: opacity 0.5s ease;
}

@media (max-width: 768px) {
  .animated-text-container {
    font-size: 2rem;
  }
}
```

### Balatro-Style Floating Animation

Gentle bobbing motion for 3D folder models:

```javascript
// Applied to each loaded 3D model
function startFloatingAnimation(model) {
  const baseY = model.position.y;
  const startTime = Date.now();
  
  function animate() {
    const time = (Date.now() - startTime) / 1000;
    
    // Vertical bobbing (sine wave)
    model.position.y = baseY + Math.sin(time * 1.2) * 0.05;
    
    // Gentle rotation
    model.rotation.y += 0.005;
    model.rotation.x = Math.sin(time * 0.8) * 0.05;
    
    // Breathing effect (scale)
    const breathe = 1 + Math.sin(time * 1.5) * 0.02;
    model.scale.set(breathe, breathe, breathe);
    
    requestAnimationFrame(animate);
  }
  
  animate();
}
```

### Hover Effects

Cards lift on hover:

```css
.interactive-card {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.interactive-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 40px var(--shadow-color);
}

.interactive-card:active {
  transform: translateY(-2px);
}
```

Button hover with glow:

```css
.button:hover {
  box-shadow: 0 0 20px var(--accent-primary);
  filter: brightness(1.1);
}
```

### Scroll Animations (GSAP)

Elements fade in when scrolling into view:

```javascript
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

gsap.from('.fade-in', {
  scrollTrigger: {
    trigger: '.fade-in',
    start: 'top 80%',
    toggleActions: 'play none none reverse'
  },
  opacity: 0,
  y: 50,
  duration: 0.8,
  stagger: 0.2
});
```

## Responsive Design

### Breakpoints

Mobile-first approach with these breakpoints:

```css
/* Extra small devices (phones) */
@media (max-width: 475px) { 
  /* Single column layouts */
}

/* Small devices (large phones, small tablets) */
@media (max-width: 640px) { 
  /* 2-column grids */
}

/* Medium devices (tablets) */
@media (max-width: 768px) { 
  /* Simplified navigation */
}

/* Large devices (small laptops) */
@media (max-width: 1024px) { 
  /* 3-column grids */
}

/* Extra large devices (desktops) */
@media (max-width: 1280px) { 
  /* 4-column grids */
}
```

### Touch Targets

All interactive elements meet 44x44px minimum (WCAG 2.1):

```css
.touch-target {
  min-width: 44px;
  min-height: 44px;
  padding: 12px;
}

/* Increase tap area on touch devices */
@media (hover: none) and (pointer: coarse) {
  .touch-target {
    padding: 16px;
    min-width: 48px;
    min-height: 48px;
  }
}
```

### Mobile Adjustments

Theme toggle shrinks on mobile:

```css
@media (max-width: 768px) {
  .theme-selector {
    top: 10px;
    right: 10px;
    padding: 6px;
    gap: 6px;
  }
  
  .theme-option {
    width: 40px;
    height: 40px;
  }
}
```

## Accessibility

### Focus Indicators

Visible focus states for keyboard navigation:

```css
.focusable:focus {
  outline: 3px solid var(--accent-primary);
  outline-offset: 2px;
  box-shadow: 0 0 0 5px rgba(102, 126, 234, 0.2);
}

/* Only show focus ring for keyboard, not mouse */
.focusable:focus:not(:focus-visible) {
  outline: none;
  box-shadow: none;
}
```

### Color Contrast

All themes meet WCAG AA standards:
- **Normal text:** 4.5:1 minimum contrast ratio
- **Large text (18pt+):** 3:1 minimum
- **Interactive elements:** Enhanced contrast for clarity

Tested with WebAIM Contrast Checker.

### High Contrast Mode

Support for system high contrast preferences:

```css
@media (prefers-contrast: high) {
  :root {
    --primary-text: #000000;
    --background-primary: #ffffff;
    --border-color: #000000;
  }
  
  .card {
    border: 3px solid var(--border-color);
  }
  
  .button {
    border-width: 3px;
  }
}
```

### Reduced Motion

Respect user's motion preferences:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Low-Spec Device Mode (New January 2026)

Automatic performance optimization for devices with limited hardware:

```css
/* Pause non-essential animations on low-spec devices */
html.low-spec-mode *,
html.reduced-motion * {
  animation-duration: 0.001ms !important;
  animation-iteration-count: 1 !important;
  transition-duration: 0.001ms !important;
}

/* Keep floating animations visually static */
html.low-spec-mode .profile-card,
html.reduced-motion .profile-card {
  animation: none !important;
}

/* Keep cursor visible but static */
html.low-spec-mode .cursor::after,
html.reduced-motion .cursor::after {
  animation: none !important;
  opacity: 1 !important;
}

/* Essential hover states still work */
html.low-spec-mode button:hover,
html.low-spec-mode a:hover,
html.low-spec-mode .tab-button:hover {
  transition-duration: 0.1s !important;
}

/* Disable expensive backdrop-filter effects */
html.low-spec-mode .hero-container,
html.low-spec-mode .profile-card,
html.low-spec-mode .playground-container {
  backdrop-filter: none !important;
}
```

The `low-spec-mode` class is automatically added to `<html>` by `performanceMode.js` when devices with ≤4 CPU cores, ≤4GB RAM, or integrated GPUs are detected.

### Touch Event Handling (New January 2026)

Mobile touch responsiveness with proper event delegation:

```javascript
// BIOS overlay button - touch support
biosButton.addEventListener('touchend', (e) => {
  e.preventDefault();
  handleBiosAction();
}, { passive: false });

// Cookie consent - touch support
acceptButton.addEventListener('touchend', (e) => {
  e.preventDefault();
  handleAccept();
}, { passive: false });
```

All interactive elements use both `click` and `touchend` handlers for reliable mobile response.

### Semantic HTML

Proper heading hierarchy and ARIA labels:

```html
<nav aria-label="Main navigation">
  <ul role="menubar">
    <li role="none">
      <a href="#projects" role="menuitem" aria-label="View projects">
        Projects
      </a>
    </li>
  </ul>
</nav>

<button aria-label="Switch to night theme" aria-pressed="false">
  <svg aria-hidden="true" focusable="false">
    <use href="#moon-icon"/>
  </svg>
</button>

<!-- Screen reader only text -->
<span class="sr-only">Current theme: Day</span>
```

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

## Design Tokens

### Spacing

Consistent spacing using 8px base:

```css
:root {
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
  --space-2xl: 48px;
  --space-3xl: 64px;
  --space-4xl: 96px;
}
```

### Border Radius

Consistent rounding:

```css
:root {
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;
  --radius-2xl: 20px;
  --radius-full: 9999px;
}
```

### Shadows

Elevation system (four levels):

```css
:root {
  --shadow-sm: 0 2px 4px var(--shadow-color);
  --shadow-md: 0 4px 8px var(--shadow-color);
  --shadow-lg: 0 8px 16px var(--shadow-color);
  --shadow-xl: 0 16px 32px var(--shadow-color);
}
```

### Z-Index Scale

Layering system:

```css
:root {
  --z-base: 1;
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-modal-backdrop: 1000;
  --z-modal: 2000;
  --z-tooltip: 3000;
  --z-notification: 4000;
}
```

## Adding New Components

When creating new components, follow this checklist:

1. ✅ Use CSS variables for all colors
2. ✅ Follow spacing tokens (8px base)
3. ✅ Include hover/focus/active states
4. ✅ Test in all 6 themes
5. ✅ Check mobile responsiveness (4 breakpoints)
6. ✅ Verify keyboard navigation works
7. ✅ Test color contrast (WCAG AA minimum)
8. ✅ Add appropriate ARIA labels
9. ✅ Respect prefers-reduced-motion
10. ✅ Use consistent transition timing (0.3s ease)

### Example Component Template

```css
.new-component {
  /* Layout */
  display: flex;
  align-items: center;
  gap: var(--space-md);
  padding: var(--space-lg);
  
  /* Colors (theme-aware) */
  background: var(--background-secondary);
  color: var(--primary-text);
  border: 1px solid var(--border-color);
  
  /* Visual polish */
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  
  /* Smooth transitions */
  transition: all 0.3s ease;
}

.new-component:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-lg);
  border-color: var(--accent-primary);
}

.new-component:focus-within {
  outline: 3px solid var(--accent-primary);
  outline-offset: 2px;
}

/* Mobile adjustments */
@media (max-width: 768px) {
  .new-component {
    flex-direction: column;
    padding: var(--space-md);
  }
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  .new-component {
    transition: none;
  }
}
```

This keeps everything consistent with the existing design system and ensures your components work seamlessly across all themes and devices.
