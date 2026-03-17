Trigger-Lock-Extension

Repository reorganized to separate source files and build output.

New frontend layout (high level):

frontend/
  src/                # development source (JS, components, background, content)
    components/
    pages/
    background/
    content/
    styles/
    assets/
  scripts/             # existing, keep until migration complete
  css/
  images/
  manifest.json        # keep for manual/extension install; can be generated during build

server/
  src/ or server.py    # server and ML model files

See `frontend/README.md` for migration steps.
