# Portfolio Overview

This portfolio started as a weekend project and grew into something much bigger. The goal was straightforward: build a personal site that actually reflects the kind of work I do—interactive, detail-oriented, and a little unconventional.

Most portfolio templates felt too generic. WordPress would have been faster, sure, but I wanted full control over every animation timing, every color transition, every interaction. So I built it from scratch using Firebase as the backend, which handles everything from blog posts to 3D model storage without needing a traditional server.

## What Makes This Different

The site leans into a retro-futuristic aesthetic—think old computer terminals mixed with modern web capabilities. Visitors are greeted with a BIOS-style loading sequence before the main content appears. It's not just decoration; it sets expectations for the kind of attention to detail they'll find throughout.

Under the hood, everything is modular. The admin dashboard handles content management. The blog system tracks engagement in real-time. The 3D showcase renders actual Blender models in the browser. Six visual themes let visitors (and me) switch the entire look with one click.

## Core Features

### The Landing Experience

When you first load the site, you're met with an animated boot sequence—command-line text appearing character by character, progress bars filling, that sort of thing. It's theatrical, intentionally so. The BIOS overlay includes a functional theme toggle button so visitors can switch themes before the main content loads. The transition into the main portfolio happens once the "system check" completes, featuring a dramatic exit animation with 3D perspective tilt and zoom scaling effect. Touch support ensures mobile users can tap to skip or interact with the loading screen.

### Hero Section

The hero borrows an idea from Claude.com: rotating text that cycles through four phrases ("CREATE & DESIGN," "FROM IDEA TO WEBSITE," and so on). The text animates with RGB color cycling at 60fps, and the colors shift automatically when you switch themes. To save battery on mobile, the animation pauses when the tab loses focus or when the section scrolls out of view.

### 3D Project Showcase

This was the most challenging part to build. Each project appears as a floating 3D folder in a grid layout (4x2 on desktop). The folders aren't static images—they're actual rendered models with subtle animations: a gentle bob, slight rotation wobble, and a breathing effect borrowed from card games like Balatro.

Clicking a folder opens a modal that spans 80% of the screen width. Inside, content is organized into tabs—each tab can hold text, images, or embedded PDFs. The camera automatically repositions to frame each model correctly, which took some trial and error to get right with Three.js.

Category filtering sits above the grid (All, Apps, Internal Projects, Modeling), and a scrolling marquee banner marks the section. The banner has an unusual behavior: it sticks to the top when you scroll down, but switches to the bottom when you scroll up. Hovering pauses the scroll so you can actually read it.

### Content Sections

The About Me, CV/Resume, and Milestones sections all pull their content from Firebase. No hardcoded text. This means I can update my bio or add a new job without touching the code.

The CV/Resume is particularly useful—categories collapse and expand, which keeps things scannable without hiding information. The milestones display as a visual timeline.

A contact form sits at the bottom, wired to EmailJS so submissions land in my inbox without a backend server. There's also a pixel disintegration effect when the section comes into view, just because it looked cool.

### The Playground

This section houses a working Pong game built on HTML5 Canvas. Beat the AI in two rounds (each round is first to three points), and you unlock a downloadable 3D model as a reward. The reward file lives in Firebase Storage, so I can swap it out anytime.

Score tracking uses square indicators that fill in as points are scored. There's a confetti explosion when you win, and a "Game Over" screen when the AI beats you. The whole thing respects the current theme—colors adapt automatically.

### Easter Eggs

Hidden in the Playground section are two mini-games that most visitors won't find unless they explore.

**Rock Breaking:** A clickable pixel-art rock sits in the corner. Click it enough times (20 total), and it starts showing cracks. The cracks appear at specific thresholds—click 5, 10, 15, 18—with particle effects bursting out each time. After the 20th click, the rock explodes. Chunks tumble to the bottom of the screen, a floating banner appears ("I TOLD YOU..."), and a dozen bugs crawl out. Flies, worms, spiders, beetles—each with their own animation quirks. You can squish them, which leaves a colored splat. Everything persists until you reload the page.

**Theme Hopper:** A star collection game where golden stars appear in each theme. Find and click the star in Day, Night, Monochrome, and Cyberpunk themes to unlock the hidden Blueprint theme. Stars feature a distinctive golden color (#FFD700) with orange stroke and glowing drop-shadow effects. A floating counter tracks progress ("X/4 Stars Found!"), and collecting all four triggers a celebration animation with falling golden stars before the Blueprint theme unlocks. The golden star icon appears next to the Blueprint theme option in the theme selector as a reward badge.

### Blog System

The blog runs on Editor.js for writing and Firebase for storage. Each post tracks views, likes, bookmarks, and shares in real-time—data that feeds into the analytics dashboard.

Visitors can leave comments (pending moderation) and reply to existing threads. The comment system supports nesting, so discussions stay organized. View counts increment on page load; likes and bookmarks require a click. All of this gets stored in Firestore, giving me a clear picture of what content resonates.

Tag-based filtering helps readers find related posts. The reading experience adapts to screen size without sacrificing typography.

### Admin Dashboard

The dashboard sits behind Google OAuth. Only whitelisted email addresses can log in—everyone else gets blocked by Firestore security rules.

Once inside, the post editor offers a dozen content block types via Editor.js: headers, paragraphs, lists, code blocks, images, tables, quotes, and more. Drafts auto-save as you type. Posts can be marked as featured, published, or kept in draft mode.

**SEO Tools**

I added real-time SEO feedback after getting tired of bouncing between the editor and external tools. As you type a title or meta description, a character counter appears in the corner. Green means you're in the sweet spot; yellow means you're pushing limits; red means something's off.

There's also a "Next Actions" card that generates specific suggestions: "Add 25 more characters to your title" or "Include a focus keyword in the first paragraph." Recommendations are prioritized—critical issues pulse red, minor tweaks show in blue.

When everything checks out, a brief celebration animation plays. It sounds silly, but it makes the editing process less tedious.

Progress bars glow when metrics hit the excellent range. Health check items scale up slightly as they pass. The overall score circle pulses when you're doing well. Subtle stuff, but it makes writing feel less like a chore.

At the top of the SEO panel, a "Next Step" banner shows the single most important thing to address. Once everything passes, it switches to a green celebration state with "All set! Ready to publish." A health check runs continuously, scoring content from 0-100 based on readability (Flesch formula), structure, tag usage, and keyword density.

**Projects Management**

The 3D project uploader accepts .obj and .mtl file pairs. A live Three.js preview appears during upload so you can verify the model looks correct before committing. The camera auto-positions based on model bounding box calculations—no manual adjustments needed.

Each project contains "folders" (really tabs) with content blocks inside: text with markdown support, images (auto-resized to 1920px max), and PDF/PPT embeds. A preview modal shows exactly how the project will appear on the public site before you save.

**Content Management**

The CV/Resume editor dynamically adapts its form fields based on category. Adding a job? You get fields for position, company, dates, and skills. Adding education? Different fields appear. Languages get proficiency dropdowns. Everything displays as cards organized by category.

Milestones feed into the portfolio's progress timeline. Each entry needs a title, date, and description. The About Me section is just a text editor with character counting and instant preview.

**Comment Moderation**

Comments land in a moderation queue. You can filter by status (pending, approved, rejected) and take quick actions on individual items or in bulk.

**Private Projects & NDA Access**

Some portfolio pieces can't be shown publicly—client work under NDA, proprietary designs, that sort of thing. These projects have a visibility toggle. When set to private, visitors see a blurred overlay with options to request access.

The NDA request flow works like this: visitor fills out a form, selects which projects they need, and signs an NDA that gets generated as a PDF via jsPDF. The request sits in a "pending" queue until I review it. If approved, the system generates temporary credentials and sends them via EmailJS. The guest can then log in and view the specific projects they were granted access to.

Authorized users are managed from a dedicated dashboard tab. I can see pending requests, approve or reject with one click, download signed NDAs, or revoke access later if needed.

**Guest Authentication**

Once approved, guests log in through a modal that validates their credentials against Firestore. Sessions have expiration dates, and a floating "NDA reminder" box stays visible while they browse protected content. Access can be scoped to specific projects or granted portfolio-wide.

**Cookie Consent**

GDPR compliance required a cookie banner. It appears on first visit, styled to match the site's aesthetic, and persists the user's choice in localStorage. Consent controls whether Firebase Analytics loads.

**Analytics**

Post-level metrics track views, likes, and engagement rates. The dashboard surfaces content quality scores and SEO performance over time.

### Theme System

The site ships with six themes, each with a distinct personality:

| Theme | Description |
|-------|-------------|
| Day | Professional light mode for typical browsing |
| Night | Dark mode with muted contrasts, easier on the eyes |
| Cyberpunk | Neon accents, high contrast, hacker vibes |
| Monochrome | Beige/tan minimalism inspired by fashion editorials |
| Blueprint | White lines on blue, like an architect's drawing |
| Retro | Warm colors, VT323 font, vintage terminal feel |

Switching themes is instant—no page reload, no flash. CSS custom properties handle the heavy lifting. Preference saves to localStorage and syncs across pages.

## Tech Stack

**Frontend**
- Vanilla JavaScript (ES6+ modules)
- Three.js for 3D rendering
- GSAP for smooth animations
- Editor.js for rich text editing
- PixiJS for pixel particle effects

**Backend**
- Firebase Firestore (database)
- Firebase Authentication (Google Sign-In)
- Firebase Storage (3D models, images, files)

**External Services**
- EmailJS for contact form submissions
- Google Fonts API (VT323, Press Start 2P)

**Hosting**
- Static web server for development
- Deployable to Firebase Hosting, Replit Static, or any static host

## Project Structure

```
portfolio-website/
├── index.html                  # Landing page (BIOS loading)
├── portfolio/                  # Main portfolio
│   ├── index.html
│   ├── config.js              # Firebase & EmailJS configuration
│   ├── css/                   # All styles
│   ├── js/                    # All scripts
│   │   ├── firebase-init.js   # Centralized Firebase initialization
│   │   ├── performanceMode.js # Adaptive performance detection
│   │   ├── guest-auth.js      # Guest authentication system
│   │   ├── nda-access.js      # NDA request form
│   │   ├── playgroundSection.js # Pong, Easter eggs, model viewer
│   │   ├── projectsBanner.js  # Scrolling marquee banner
│   │   ├── scrollLock.js      # Modal scroll lock utility
│   │   └── ...                # Other modules
│   ├── models/                # 3D assets (.obj, .mtl)
│   └── textures/              # Material textures
├── blog/                       # Public blog
│   ├── index.html
│   ├── post.html
│   ├── css/
│   └── js/
├── admin/                      # Admin dashboard
│   ├── index.html
│   ├── css/                   # Admin styles
│   ├── js/                    # Admin modules
│   └── editorjs/              # Editor plugins
├── landing/                    # BIOS loading screen
│   ├── index.html
│   ├── css/
│   └── js/
├── shared/                     # Shared components
│   ├── css/                   # Global themes, footer, scrollbar, cookie consent
│   └── js/                    # Theme toggle, cookie consent, utilities
└── documentation/             # Technical docs
```

## Firebase Collections

| Collection | Purpose | Access Control |
|------------|---------|----------------|
| `blogPosts` | Blog content and metadata | Public read, admin write |
| `blogComments` | User comments with moderation status | Public read, anyone create, admin moderate |
| `interactions` | Views, likes, bookmarks, shares | Public read/write |
| `contacts` | Contact form submissions | Anyone create, admin read |
| `milestones` | Progress bar milestones | Public read, admin write |
| `aboutMe` | About section content (single document) | Public read, admin write |
| `resume` | CV items organized by category | Public read, admin write |
| `projects` | Portfolio projects with 3D models | Public/Private visibility, admin write |
| `ndaAccessRequests` | NDA access request submissions | Public create, admin manage |
| `authorizedGuests` | Approved guest credentials | Admin only |
| `guestSessions` | Active guest login sessions | Public create/read, admin manage |

## How the 3D System Works

Projects display custom 3D models created in Blender and exported as .obj/.mtl files.

**Workflow:**
1. Create model in Blender
2. Export as Wavefront .obj (with materials, Y-up, triangulated)
3. Upload both .obj and .mtl files via admin dashboard
4. Three.js renders the model in the portfolio grid
5. Models stored in Firebase Storage (`projects/models/`)
6. Cached globally to prevent reloading on category filter

**Optimizations:**
- Lazy loading with IntersectionObserver (only load visible models)
- Global model caching (90% faster category filtering)
- WebGL memory management (proper cleanup on filter/modal close)
- Smart camera positioning (automatic model centering)

Each project can have multiple folders containing mixed content blocks:
- Text content (markdown formatting)
- Images (auto-optimized, max 1920px width)
- PDFs or PowerPoint files

The folders become tabs in the project modal viewer (80% screen width for optimal viewing on all devices).

## Theme Implementation

Themes use CSS custom properties for instant color switching.

**How it works:**
1. Inline script in `<head>` loads saved theme from localStorage
2. Applies theme class to `<html>` before page renders
3. Zero flash of unstyled content (FOUC)
4. Syncs across all pages via localStorage
5. Unified theme toggle component on every page

All six themes are available by default—no unlock mechanism required.

## SEO & Analytics

The admin dashboard provides comprehensive SEO tools that update in real-time as you write:

**SEO Scoring:**
- Title optimization (30-60 characters optimal)
- Meta description optimization (120-160 characters optimal)
- Content structure analysis (headers, lists, media)
- Readability scoring (Flesch Reading Ease formula)
- Tag usage validation (2-5 tags recommended)

**Enhanced Features (New October 2025):**
- Live character counters with color-coded feedback
- Dynamic "Next Actions" with priority-based recommendations
- Visual celebration effects when metrics hit optimal ranges
- Prominent "Next Step" banner showing single most important action
- Real-time progress bar updates with smooth animations

**Analytics:**
- Firebase interaction tracking (actual views/likes, not mocked)
- Engagement rate calculation
- Content quality metrics
- Per-post performance data

All SEO features work across all six themes with instant theme reactivity.

## Performance Optimizations

**Adaptive Performance Mode (Updated January 2026):**
- Automatic device capability detection using weighted signal system
- Weighted detection: cores ≤2 (+2 signals), memory ≤2GB (+2), weak GPU without discrete identifier (+2), mobile with ≤4 cores (+1)
- Low-spec mode triggers at 3+ weak signals (prevents false positives)
- Correctly identifies discrete GPUs (NVIDIA RTX, AMD Radeon) as high-performance
- Respects `prefers-reduced-motion` system preference
- CSS animations paused on low-spec devices (excludes BIOS overlay animations)
- `backdrop-filter` effects disabled on weak hardware
- 3D renderers limited to max 2 concurrent on low-spec devices
- FPS capped at 30 (vs 60) on low-power hardware
- WebGL antialiasing disabled, pixel ratio capped to 1

**Zoom Compensation (New January 2026):**
- Detects browser/system zoom levels above 110%
- Distinguishes HiDPI/Retina displays from actual zoom using screen resolution
- Counter-scales the page using CSS transform to maintain intended layout
- Minimum compensation at 65% scale for very high zoom levels
- User can disable via localStorage preference
- Works with pixel disintegration effect (grid rebuilds on zoom change)

**Loading & Rendering:**
- Theme applied in `<head>` to prevent flash
- Lazy loading for images and 3D models with IntersectionObserver
- Visibility-aware animations (pause when tab hidden or section offscreen)
- Code splitting for admin modules
- WebP images with fallbacks
- Passive event listeners for scroll/touch

**Firebase (First-Visit Optimization):**
- SDK loaded in `<head>` with preconnect hints for immediate availability
- Network-first strategy on first visit to ensure "WAU effect" (content visible immediately)
- Cache-first strategy on return visits for faster loading
- Centralized initialization (prevents duplicate app instances)
- Query limits (20 projects max) with localStorage caching
- Offline error handling with fallback to cached data

**3D Performance:**
- Three.js asset caching (models cached globally, prevent reload on filter)
- WebGL memory management (proper renderer disposal)
- Lazy loading (only render visible folders)
- Optimized model scaling (1.5x ratio)
- Performance-aware renderer settings (antialias, pixelRatio, powerPreference)

**Touch Responsiveness:**
- Touch event handlers (`touchend`) on BIOS overlay and cookie consent
- Proper event delegation for mobile interactions
- All interactive elements meet 44x44px minimum tap targets

**Animations:**
- Hero text animation pauses when tab hidden or section offscreen
- RequestAnimationFrame for smooth 60fps animations (30fps on low-spec)
- RGB color cycling optimized (removed unnecessary getComputedStyle calls)

## Why I Built It This Way

Instead of using a CMS or template, I wanted:
- Complete design control over every element
- Custom 3D integration for showcasing Blender work
- A learning project for Firebase and modern web APIs
- Portfolio and blog in one cohesive platform
- Admin tools tailored specifically to my workflow
- Real-time SEO feedback to improve my writing

The retro-futuristic theme reflects my design style while staying professional enough for client work and job applications.

## Use Cases

**For Visitors:**
- Browse portfolio projects with interactive 3D previews
- Read blog posts with full-text search and filtering
- Switch between six visual themes to match preference
- Download or view my resume/CV
- Contact me via integrated form
- Engage with content (like, bookmark, comment)
- Request NDA access to view private/confidential projects
- Login as guest with approved credentials

**For Me (Admin):**
- Write blog posts with rich formatting and real-time SEO feedback
- Upload 3D models and manage project files
- Set project visibility (public/private)
- Moderate user comments
- Track post performance and engagement
- Update resume and about section
- Manage all content from one centralized dashboard
- Get actionable recommendations to improve SEO
- Review and approve NDA access requests
- Manage authorized guests and their access

## Current Status

**Production Ready (100%)**
- ✅ All core features implemented and functional
- ✅ Firebase fully integrated
- ✅ Admin dashboard with enhanced SEO tools complete
- ✅ 3D model system optimized and stable
- ✅ Blog system with comments and moderation
- ✅ Six themes with instant switching
- ✅ Performance optimized (lazy loading, caching, visibility-aware animations)
- ✅ Private projects with NDA access system
- ✅ Guest authentication with temporary passwords
- ✅ PDF generation and email notifications
- ✅ Ready to deploy (see `DEPLOYMENT_GUIDE.md` and `NDA-FEATURE-SETUP.md`)

## Next Steps

The site is production-ready and includes everything needed for professional use. Future enhancements could include:
- More Editor.js plugins (video embeds, image galleries)
- Advanced 3D viewer (texture swapping, lighting controls)
- Blog post scheduling system
- Email notifications for new comments
- Analytics dashboard with charts and graphs
- Multi-language support
- Progressive Web App (PWA) capabilities

But for now, it accomplishes exactly what I need: a professional platform for showcasing my work and sharing technical knowledge through well-optimized blog posts.
