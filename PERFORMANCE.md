# AstroWind Helper - Performance Optimizations

This template includes several performance optimizations based on Google PageSpeed Insights analysis and real-world testing.

## ✅ Built-in Optimizations

### 1. Inline CSS for Faster Initial Load
**Location:** `astro.config.ts`

```typescript
build: {
  inlineStylesheets: 'always',
}
```

**Benefit:** Reduces HTTP requests and improves First Contentful Paint (FCP) by inlining critical CSS directly in HTML.

### 2. Asset Compression
**Location:** `astro.config.ts`

```typescript
compress({
  CSS: true,
  HTML: {
    'html-minifier-terser': {
      removeAttributeQuotes: false,
    },
  },
  Image: false, // Handled by Astro's built-in optimization
  JavaScript: true,
  SVG: false,
  Logger: 1,
})
```

**Benefit:** Automatic minification of CSS, HTML, and JavaScript reduces file sizes by 30-50%.

### 3. Lazy Loading Images
**Location:** `src/utils/frontmatter.ts`

The template includes a `lazyImagesRehypePlugin` that automatically adds lazy loading to images in markdown content.

**Benefit:** Improves initial page load time by deferring off-screen images.

### 4. Responsive Images
**Location:** Uses Astro's built-in image optimization

**Benefit:** Serves appropriately sized images based on device, reducing bandwidth usage.

### 5. Self-Hosted Fonts
**Location:** Uses `@fontsource` packages

**Benefit:** Eliminates external font requests to Google Fonts, improving privacy and reducing DNS lookups.

---

## 🚀 Deployment-Specific Optimizations

### Cloudflare Pages Deployment

For maximum performance on Cloudflare Pages, uncomment the adapter in `astro.config.ts`:

```typescript
import cloudflare from '@astrojs/cloudflare';

export default defineConfig({
  adapter: cloudflare({
    platformProxy: {
      enabled: true,
    },
    imageService: 'compile',
  }),
  output: 'server', // or 'hybrid'
  // ... rest of config
});
```

**Installation:**
```bash
npm install @astrojs/cloudflare
```

**Benefits:**
- Edge caching for faster global delivery
- Optimized image serving
- Server-side rendering capabilities (if needed)

---

## 📊 Performance Checklist for New Projects

When starting a new project from this template, follow these steps to ensure optimal performance:

### Before Deployment

- [ ] **Optimize Icons**
  - Review `astro.config.ts` icon configuration
  - Replace `tabler: ['*']` with only the icons you actually use
  - Example: `tabler: ['home', 'mail', 'phone']`
  - **Potential savings:** 100-200KB

- [ ] **Remove Unused Images**
  - Delete any template images you're not using
  - Convert PNGs to WebP format
  - Use tools like [Squoosh](https://squoosh.app/) for compression

- [ ] **Optimize Videos (if using)**
  - Compress video files to <1MB for hero sections
  - Use tools like [HandBrake](https://handbrake.fr/) or FFmpeg
  - Add poster images for videos
  - Consider lazy-loading videos below the fold

- [ ] **Check Font Loading**
  - Ensure fonts are only imported once (check Layout.astro)
  - Avoid duplicate font imports in components and layouts

- [ ] **Enable Image Compression**
  - If deploying to non-Cloudflare host, consider enabling:
    ```typescript
    compress({
      Image: true,
      // ... other settings
    })
    ```

### Performance Testing

1. **Run Lighthouse Audit**
   - Use Chrome DevTools > Lighthouse
   - Test in Incognito mode for accurate results
   - Aim for 90+ scores across all metrics

2. **Test with Google PageSpeed Insights**
   - Visit [PageSpeed Insights](https://pagespeed.web.dev/)
   - Test both mobile and desktop
   - Address any Critical or High-priority issues

3. **Check Core Web Vitals**
   - Largest Contentful Paint (LCP): < 2.5s
   - First Input Delay (FID): < 100ms
   - Cumulative Layout Shift (CLS): < 0.1

---

## 🎯 Common Performance Issues & Solutions

### Issue: Slow Initial Page Load

**Solutions:**
- Enable `inlineStylesheets: 'always'` ✅ (already enabled)
- Minimize above-the-fold images
- Use `loading="eager"` for hero images only
- Lazy load everything else

### Issue: Large Bundle Size

**Solutions:**
- Optimize icon imports (use specific icons only)
- Remove unused dependencies
- Enable tree-shaking in build process
- Code-split large components

### Issue: Poor Mobile Performance

**Solutions:**
- Use responsive images with appropriate sizes
- Reduce video quality for mobile
- Test on real mobile devices, not just DevTools
- Consider using `<picture>` element for different device sizes

### Issue: Slow Third-Party Scripts

**Solutions:**
- Use Partytown for analytics scripts ✅ (available in config)
- Defer non-critical scripts
- Load Google Analytics via Partytown
- Self-host third-party resources when possible

---

## 📈 Performance Metrics from Real Testing

Based on the rebirthwd project (built from this template):

### Before Optimization:
- Page Weight: ~2-3MB
- LCP: 3-4s
- FCP: 2-3s
- Lighthouse Score: 75-80

### After Optimization:
- Page Weight: ~500KB-1MB
- LCP: 1.5-2s
- FCP: 0.8-1.2s
- Lighthouse Score: 95-100

**Key Changes Applied:**
1. Added `inlineStylesheets: 'always'`
2. Optimized icon loading
3. Compressed images to WebP
4. Removed duplicate font imports
5. Lazy-loaded off-screen content

---

## 🛠️ Recommended Tools

### Image Optimization
- [Squoosh](https://squoosh.app/) - Web-based image compression
- [ImageOptim](https://imageoptim.com/) - Mac app for batch optimization
- [Sharp](https://sharp.pixelplumbing.com/) - Already included via Astro

### Video Compression
- [HandBrake](https://handbrake.fr/) - Free video transcoder
- [FFmpeg](https://ffmpeg.org/) - Command-line video processing

### Testing
- [Google PageSpeed Insights](https://pagespeed.web.dev/)
- Chrome DevTools Lighthouse
- [WebPageTest](https://www.webpagetest.org/)
- [GTmetrix](https://gtmetrix.com/)

### Bundle Analysis
```bash
npm run build -- --verbose
```

---

## 📚 Additional Resources

- [Astro Performance Guide](https://docs.astro.build/en/guides/performance/)
- [Core Web Vitals](https://web.dev/vitals/)
- [Image Optimization Best Practices](https://web.dev/fast/#optimize-your-images)
- [Cloudflare Pages Optimization](https://developers.cloudflare.com/pages/platform/limits/)

---

## 🤝 Contributing Performance Improvements

If you discover additional performance optimizations, please:
1. Test thoroughly with PageSpeed Insights
2. Document the improvement with before/after metrics
3. Submit a PR or open an issue
4. Help make this template even faster!

---

**Last Updated:** January 2026
**Template Version:** Based on AstroWind 1.0.0-beta.52
