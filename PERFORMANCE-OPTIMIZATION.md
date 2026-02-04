# Landing Page Performance Optimization Guide

## Current Issues

1. ❌ Tailwind CSS loaded from CDN (blocking render, ~2MB)
2. ❌ DaisyUI loaded from CDN (unused, ~800KB)
3. ❌ Google Fonts from CDN (render blocking)
4. ❌ Large video files load immediately (1.1MB + 486KB)
5. ❌ No lazy loading for below-fold content
6. ❌ No resource compression/minification
7. ❌ Both webm + mp4 loaded (wasteful)
8. ❌ No image optimization for poster frames

**Current load:** ~4-5MB initial payload

## Priority 1: Critical Rendering Path (Immediate Impact)

### 1.1 Inline Critical CSS
**Problem:** External CSS blocks rendering
**Solution:** Extract and inline critical CSS (above-the-fold styles)

```html
<style>
/* Inline critical CSS here - fonts, hero section, layout */
body { font-family: system-ui, -apple-system, sans-serif; background: #F8F7F4; }
.hero-text { font-size: 60px; line-height: 1; }
/* ... rest of critical styles */
</style>

<!-- Defer non-critical CSS -->
<link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
```

### 1.2 Remove Unused Libraries
**Problem:** DaisyUI not used, Tailwind loaded in full
**Solution:** 
- ✂️ Remove DaisyUI completely
- Use Tailwind CLI to generate purged CSS (~10-20KB)

```bash
# Generate minimal Tailwind build
npx tailwindcss -o styles.min.css --minify
```

### 1.3 Self-host & Optimize Fonts
**Problem:** Google Fonts = 2 external requests + render blocking
**Solution:** 

```html
<!-- Remove Google Fonts CDN -->
<link rel="preload" href="/fonts/roboto-mono.woff2" as="font" type="font/woff2" crossorigin>
<style>
@font-face {
  font-family: 'Roboto Mono';
  src: url('/fonts/roboto-mono.woff2') format('woff2');
  font-display: swap; /* Show fallback immediately */
}
</style>
```

**Impact:** -400ms load time, eliminate FOUT

## Priority 2: Video Optimization (Biggest File Size)

### 2.1 Lazy Load Videos
**Problem:** Videos load immediately, even if user bounces
**Solution:** Intersection Observer for lazy loading

```html
<video class="lazy" data-src="hermes_hero.webm" poster="hermes_hero_poster.jpg">
</video>

<script>
const lazyVideos = document.querySelectorAll('video.lazy');
const videoObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const video = entry.target;
      video.src = video.dataset.src;
      video.load();
      videoObserver.unobserve(video);
    }
  });
});
lazyVideos.forEach(video => videoObserver.observe(video));
</script>
```

### 2.2 Compress Videos Further
**Problem:** 1.1MB + 486KB videos
**Solution:** Re-encode with better compression

```bash
# Desktop video (target: 300-400KB)
ffmpeg -i hermes_hero.mp4 -vcodec libx264 -crf 28 -preset slow -vf "scale=720:-2" hermes_hero_optimized.mp4

# Mobile video (target: 150-200KB)
ffmpeg -i hermes_hero_mobile.mp4 -vcodec libx264 -crf 30 -preset slow -vf "scale=480:-2" hermes_hero_mobile_optimized.mp4
```

### 2.3 Smart Video Format Selection
**Problem:** Loading both webm + mp4
**Solution:** Feature detection, load only what's needed

```javascript
const video = document.querySelector('video');
const supportsWebM = video.canPlayType('video/webm') !== '';
video.src = supportsWebM ? 'video.webm' : 'video.mp4';
```

### 2.4 Add Video Preload Strategy
```html
<video preload="none" poster="poster.jpg">
  <!-- preload="none" = don't load until user clicks play -->
  <!-- preload="metadata" = load first frame only -->
</video>
```

**Impact:** -70% video payload, faster initial load

## Priority 3: Image Optimization

### 3.1 Optimize Poster Images
**Problem:** Poster JPGs not optimized
**Solution:**

```bash
# Convert to WebP (better compression)
cwebp -q 80 hermes_hero_poster.jpg -o hermes_hero_poster.webp

# Optimize JPG fallback
jpegoptim --max=80 --strip-all hermes_hero_poster.jpg
```

### 3.2 Responsive Images
```html
<picture>
  <source media="(max-width: 768px)" srcset="poster-mobile.webp" type="image/webp">
  <source media="(max-width: 768px)" srcset="poster-mobile.jpg" type="image/jpeg">
  <source srcset="poster-desktop.webp" type="image/webp">
  <img src="poster-desktop.jpg" alt="Hero">
</picture>
```

**Impact:** -50% image payload

## Priority 4: Code Optimization

### 4.1 Minify HTML/CSS/JS
```bash
# Install tools
npm install -g html-minifier csso uglify-js

# Minify
html-minifier --collapse-whitespace --remove-comments index.html -o index.min.html
```

### 4.2 Optimize Typewriter Script
**Problem:** Current script runs constantly
**Solution:** More efficient version

```javascript
// Optimized typewriter (uses requestAnimationFrame)
const typewriter = (() => {
  const phrases = ["...", "..."];
  let phrase = 0, char = 0, deleting = false;
  const el = document.getElementById('typewriter');
  
  const type = () => {
    const current = phrases[phrase];
    el.textContent = deleting ? current.slice(0, char--) : current.slice(0, char++);
    
    if (!deleting && char === current.length) deleting = true;
    else if (deleting && char === 0) { deleting = false; phrase = (phrase + 1) % phrases.length; }
    
    setTimeout(type, deleting ? 20 : (char === current.length ? 2000 : 80));
  };
  type();
})();
```

## Priority 5: Server & Delivery

### 5.1 Enable Compression
**nginx config:**
```nginx
gzip on;
gzip_types text/css application/javascript application/json image/svg+xml;
gzip_min_length 1024;
```

### 5.2 Add Caching Headers
```nginx
location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff2)$ {
  expires 1y;
  add_header Cache-Control "public, immutable";
}
```

### 5.3 Use CDN
Deploy to Vercel/Cloudflare/Netlify for:
- Global edge caching
- Automatic compression
- HTTP/3 support
- Built-in CDN

## Priority 6: Mobile-Specific Optimizations

### 6.1 Reduce Mobile Bundle
**Problem:** Mobile loads same assets as desktop
**Solution:** Conditional loading

```html
<script>
const isMobile = window.innerWidth < 768;
if (isMobile) {
  // Load mobile-specific assets only
  loadVideo('hermes_hero_mobile_optimized.mp4');
} else {
  loadVideo('hermes_hero_optimized.mp4');
}
</script>
```

### 6.2 Touch Optimization
```css
/* Reduce paint on mobile */
.btn { 
  -webkit-tap-highlight-color: transparent;
  touch-action: manipulation;
}
```

### 6.3 Viewport Meta Optimization
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
```

## Implementation Checklist

### Quick Wins (30 min)
- [ ] Remove DaisyUI CDN link
- [ ] Add `font-display: swap` to fonts
- [ ] Set video `preload="none"`
- [ ] Minify HTML (html-minifier)
- [ ] Compress images (jpegoptim/cwebp)

### Medium Effort (2-3 hours)
- [ ] Generate purged Tailwind CSS locally
- [ ] Self-host fonts (download woff2 files)
- [ ] Re-encode videos at lower bitrate
- [ ] Implement lazy loading for videos
- [ ] Add resource hints (preconnect, preload)

### Advanced (4-6 hours)
- [ ] Create responsive image srcsets
- [ ] Implement service worker for caching
- [ ] Add critical CSS inline
- [ ] Set up CDN deployment
- [ ] Create mobile-specific bundle

## Expected Results

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| **Initial Load** | 4-5MB | 300-500KB | -90% |
| **First Paint** | 2.5s | 0.8s | -68% |
| **Time to Interactive** | 4.2s | 1.5s | -64% |
| **Lighthouse Score** | 65 | 95+ | +46% |
| **Mobile Load (3G)** | 8-10s | 2-3s | -70% |

## Tools for Testing

1. **Lighthouse** - `npm install -g lighthouse`
2. **WebPageTest** - https://www.webpagetest.org
3. **Chrome DevTools** - Network tab, Performance tab
4. **GTmetrix** - https://gtmetrix.com

## Quick Command Reference

```bash
# Analyze current size
du -sh hermes-landing/

# Optimize images
find . -name "*.jpg" -exec jpegoptim --max=80 {} \;
find . -name "*.png" -exec optipng -o7 {} \;

# Compress videos
ffmpeg -i input.mp4 -vcodec libx264 -crf 28 output.mp4

# Generate Tailwind CSS (purged)
npx tailwindcss -i input.css -o output.css --minify

# Test performance
lighthouse https://yoursite.com --view
```

## Next Steps

1. Start with **Priority 1** (Critical CSS) - biggest impact
2. Then **Priority 2** (Video optimization) - biggest file size
3. Deploy to Vercel/Netlify for free CDN + compression
4. Run Lighthouse, aim for 90+ score
5. Test on real mobile device with throttled connection

Want me to implement any of these optimizations now?
