Frontend folder re-organization

This frontend folder now contains a `src/` directory for source files and an assets/styles structure to make bundling and development easier.

Suggested next steps:
- Move JS source files from `scripts/`, `css/` and root into `src/` (preserve originals for now).
- Install a bundler (Vite or Webpack) and update `package.json` build scripts.
- Keep `manifest.json` at the frontend root for Chrome/Firefox to load during development; create a build step that copies `src/manifest.json` to the root `dist/` as needed.
