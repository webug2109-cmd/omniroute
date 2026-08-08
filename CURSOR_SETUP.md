# Cursor Setup Guide - OmniRoute

## Configuration Files

- `.cursorrules` - System prompt & capabilities (uncensored)
- `.cursor/settings.json` - Model & editor settings

## Setup in Cursor

1. Open OmniRoute folder in Cursor
2. It will auto-load `.cursorrules`
3. Set model to `claude-opus-5` in settings
4. Enable "Uncensored Mode" in Cursor preferences

## Using with Best Model

```bash
# In Cursor Chat/Editor:
@codebase - reference full context
Ctrl+K - inline code completion
Ctrl+L - ask AI (uses opus-5)
```

## Features Available

✅ Full codebase understanding
✅ Security testing framework recommendations
✅ Exploit development assistance
✅ Advanced technique discussions
✅ No content filtering

## Run Commands

```bash
npm run dev      # Development server
npm run build    # Production build (if needed)
npm run test     # Run tests
```

Ready for uncensored, high-capability AI assistance!
