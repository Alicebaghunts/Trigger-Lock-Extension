Source folder for frontend development

Place development source files here and update import paths during migration. Suggested structure:
- src/components/  -> UI components (move from scripts/components)
- src/pages/       -> popup, options, other extension pages
- src/background/  -> background service worker / background scripts
- src/content/     -> content scripts
- src/styles/      -> CSS and preprocessors
- src/assets/      -> images and static assets

Initial migration plan:
1. Move files from `frontend/scripts/components` into `frontend/src/components`.
2. Move `popup.js` into `src/pages/popup.js`, `content.js` into `src/content/content.js`, `background.js` into `src/background/background.js`.
3. Move `css/style.css` into `src/styles/style.css`.
4. Update bundler config and local `manifest.json` if you change paths for runtime assets.
