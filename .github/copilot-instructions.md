# Copilot instructions for RedLion site

## Project overview
- Static marketing site for Red Lion Advisory based on the Crafto HTML5/SCSS template.
- No Node/bundler tooling is checked in; pages are plain HTML in the repo root (e.g. [index.html](index.html), [services.html](services.html), [case-studies.html](case-studies.html)).
- Styling is authored in SCSS under [sass](sass) and compiled CSS is in [css](css); several `.min.css` files are generated artifacts.
- Frontend behavior is jQuery-based and lives mainly in [js/main.js](js/main.js) plus vendor scripts in [js/vendors](js/vendors) and [js/vendors.js](js/vendors.js).
- Email handling is done with PHP + PHPMailer in [email-templates](email-templates).

## Frontend structure and patterns
- Treat [sass/style.scss](sass/style.scss) as the main SCSS entry that imports all partials under sass/core, sass/header, sass/elements, etc.
- When adjusting global or repeated styling, prefer editing the appropriate SCSS partials (for example, button styles, grids, and typography) and then compiling to CSS rather than editing `.min.css` files.
- Small, page-specific tweaks can go into [css/style.css](css/style.css) or a dedicated non-minified CSS file; avoid touching `*.min.css` unless fixing a production-only bug.
- JavaScript is structured as a single large IIFE in [js/main.js](js/main.js) that wires up navigation, animations, and plugin initialization using jQuery and data attributes.
- Follow the existing jQuery patterns (event delegation on `$(document)`, use of utility functions, breakpoint constants such as `menuBreakPoint`) when adding new UI behaviors.
- Keep new code compatible with the existing non-module setup (no ES module imports or bundlers); add new dependencies only if they can be included as standalone scripts like the existing vendors.

## Forms, email, and backend
- Contact and newsletter forms post to handlers under [email-templates](email-templates); the primary contact handler is [email-templates/contact-form.php](email-templates/contact-form.php).
- The top PHPMailer-based block in contact-form.php is the active logic for Red Lion (sending from `info@redlionadv.com` to team addresses); the longer, more generic template logic after the first `exit;` is legacy from the original theme and is effectively dead code.
- When changing email behavior (recipients, subject, SMTP settings), update only the active PHPMailer section unless you intentionally revive more of the template logic.
- Assume a simple PHP environment using `localhost:25` with unauthenticated SMTP; do not introduce framework dependencies (e.g., Laravel, Symfony) without explicit instruction.

## Assets and third‑party integrations
- Images live under [images](images) (notably [images/redLine](images/redLine)); keep paths stable when renaming or optimizing assets and update all HTML/JS references together.
- Custom fonts are stored in [myfonts](myfonts) and [fonts](fonts); reuse existing font families where possible instead of adding new ones.
- The Revolution Slider integration and its assets are under [revolution](revolution); treat this as a third‑party plugin and avoid modifying its core JS/CSS unless you are fixing a specific bug.
- Many visual effects rely on vendor scripts in [js/vendors](js/vendors) (e.g., GSAP, Isotope, magnific-popup); initialize new components by following the existing data-attribute and initialization patterns in main.js.

## Conventions and cautions for AI agents
- Keep the stack lightweight: use plain HTML/SCSS/JS (with jQuery) and PHP; do not introduce SPA frameworks or heavy build pipelines.
- Prefer modifying non-minified sources (SCSS, unminified JS) and allow humans or external tooling to regenerate minified assets.
- Preserve the existing class naming and data-attribute conventions from the Crafto template to avoid breaking layout or JS hooks.
- When adding new pages, clone an existing HTML file that is closest in layout and adjust content and sections rather than starting from a blank document.
- Be cautious when editing long, shared files like js/main.js; changes there can affect many pages. Keep new logic well-scoped and consistent with surrounding patterns.
