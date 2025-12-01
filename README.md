# Browser Compatibility Checker

A universal browser compatibility checker built with vanilla HTML/CSS/JavaScript. Works on all browsers, even very old ones, to detect modern web feature support.

## Features

### General Browser Features
- WebGL 1.0 & 2.0
- Web Audio API
- Local Storage
- Promises
- ES6 Features
- Fetch API
- CSS Transforms, Transitions, Animations
- CSS Grid & Variables
- Flexbox
- Service Workers

### Cloudflare Stream Video Requirements
- WebRTC (adaptive streaming)
- Media Source Extensions (MSE)
- H.264, HEVC/H.265, VP9, AV1 video codecs
- Fullscreen API
- Iframe embedding support
- Browser isolation detection

## Deployment

### Build for Cloudflare Pages

```bash
npm run build
```

This creates a `dist` folder with the static HTML file.

### Cloudflare Pages Settings

- **Build command**: `npm run build`
- **Build output directory**: `dist`
- **Deploy command**: Leave empty (or use `npx wrangler deploy` with wrangler.toml)

## Local Testing

Simply open `public/index.html` in any browser. No build step or server required for local testing.

## Browser Compatibility

Written in ES5 JavaScript with traditional CSS (no Grid/Flexbox in layout) to ensure it runs on:
- Internet Explorer 9+
- All modern browsers
- Browser isolation environments
- Very old versions of Chrome, Firefox, Safari

The checker itself will work even on browsers that fail most feature checks!
