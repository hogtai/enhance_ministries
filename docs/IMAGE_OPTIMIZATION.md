# Image Optimization Guide

This guide explains how to optimize images for the Enhance Ministries website to improve Lighthouse performance scores and page load times.

## Current Problem

The website currently has **8.8 MB** of images being loaded, causing:
- **NO_LCP error** - Largest Contentful Paint fails because hero image takes too long
- **Slow mobile loading** - Mobile users on slower connections experience delays
- **Poor Core Web Vitals** - Affects SEO ranking

## Images That Need Optimization

### Critical (Do First)

| Image | Current Size | Target Size | Action |
|-------|-------------|-------------|--------|
| `hero-mountain.jpg` | **1,687 KB** | < 150 KB | Resize to 1920x1080, WebP, quality 75-80 |
| `matt-headshot.jpeg` | 484 KB | < 50 KB | Resize to 400x400, WebP |
| `matt-story.png` | 285 KB | < 80 KB | Resize to 500px wide, WebP |

### Team Photos

| Image | Current Size | Target Size | Action |
|-------|-------------|-------------|--------|
| `al-anderstrom.jpg` | 284 KB | < 30 KB | Resize to 300x300, WebP |
| `adrianna-frelich.jpg` | 237 KB | < 30 KB | Resize to 300x300, WebP |
| `tait-hoglund-new.png` | 163 KB | < 30 KB | Resize to 300x300, WebP |
| `cj-lloyd.jpg` | 43 KB | < 20 KB | Resize to 300x300, WebP |

### Other Large Images

| Image | Current Size | Target Size | Notes |
|-------|-------------|-------------|-------|
| `mission-trip-2.png` | **3,323 KB** | < 200 KB | Extremely oversized |
| `mission-trip-1.png` | **2,005 KB** | < 200 KB | Extremely oversized |
| `book-sunday-school.jpg` | 692 KB | < 100 KB | |
| `hero-about.png` | 662 KB | < 150 KB | |
| `matt-speaking.png` | 442 KB | < 80 KB | |

### Logo

| Image | Current Size | Target Size | Action |
|-------|-------------|-------------|--------|
| `logo.png` | 42 KB | < 10 KB | Resize to 300px wide, PNG-8 or WebP |

## How to Optimize Images

### Option 1: Squoosh (Recommended - Free, Browser-Based)

1. Go to [squoosh.app](https://squoosh.app/)
2. Drag and drop an image
3. On the right side:
   - Select **WebP** format
   - Set **Quality** to 75-80
   - Enable **Resize** and set dimensions (see table above)
4. Click **Download** (bottom right, blue arrow icon)
5. Replace the original file in the `assets/` folder

### Option 2: TinyPNG/TinyJPG (Free, Batch Processing)

1. Go to [tinypng.com](https://tinypng.com/)
2. Upload images (up to 20 at a time, free)
3. Download compressed versions
4. Note: TinyPNG doesn't resize - resize first using another tool

### Option 3: ImageOptim (Mac) / FileOptimizer (Windows)

- **Mac**: Download [ImageOptim](https://imageoptim.com/) (free)
- **Windows**: Download [FileOptimizer](https://nikkhokkho.sourceforge.io/static.php?page=FileOptimizer)
- Drag and drop images for automatic optimization

### Option 4: Command Line (Advanced)

If you have ImageMagick installed:

```bash
# Convert and resize hero image to WebP
convert assets/hero-mountain.jpg -resize 1920x1080 -quality 75 assets/hero-mountain.webp

# Batch convert team photos
for img in assets/matt-headshot.jpeg assets/al-anderstrom.jpg assets/adrianna-frelich.jpg; do
  convert "$img" -resize 400x400 -quality 80 "${img%.*}.webp"
done
```

## After Optimization

### Update HTML to Use WebP (with Fallback)

If you create WebP versions, update HTML to use them with JPEG/PNG fallback:

```html
<picture>
  <source srcset="assets/hero-mountain.webp" type="image/webp">
  <img src="assets/hero-mountain.jpg" alt="..." width="1920" height="1080">
</picture>
```

Or simply replace the original files with optimized versions (keeping the same filename).

### Verify Results

After optimization, check:
1. Total `assets/` folder size should be under 2 MB (ideally under 1 MB)
2. Run Lighthouse again to verify LCP and performance improvements
3. Test on mobile to ensure images look acceptable quality

## Expected Results

After optimization:

| Metric | Before | After (Expected) |
|--------|--------|------------------|
| Total image size | 8.8 MB | < 1 MB |
| LCP | Error (NO_LCP) | < 2.5s |
| Performance Score | Failed | 80+ |

## Tips

1. **WebP is ~25-35% smaller** than JPEG at the same quality
2. **Don't over-compress** - quality 70-80 is usually the sweet spot
3. **Test on retina displays** - images at 2x display size look better on high-DPI screens
4. **Use responsive images** for different screen sizes (optional, advanced)

## Resources

- [Squoosh.app](https://squoosh.app/) - Best free browser-based optimizer
- [web.dev Image Optimization Guide](https://web.dev/fast/#optimize-your-images)
- [WebP Browser Support](https://caniuse.com/webp) - 97%+ browser support
