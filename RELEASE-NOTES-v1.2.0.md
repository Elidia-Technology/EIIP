# EIIP v1.2.0 Release Notes

**Release Date:** October 2024  
**Package:** eiip@1.2.0

## 🎉 What's New in v1.2.0

This release adds **5 powerful new classes** with advanced image manipulation capabilities, all processing client-side in the browser!

### 🆕 New Classes

#### 1. **ImageResizer** - Resize & Scale Images
- **Methods:**
  - `resize(input, options)` - Resize images to specific dimensions
  - `scale(input, scaleFactor, options)` - Scale images by percentage
  - `createThumbnail(input, options)` - Create thumbnails automatically
  
- **Features:**
  - Multiple fit modes: `contain`, `cover`, `fill`
  - High-quality image smoothing
  - Smart aspect ratio preservation
  - Custom quality control
  
- **Fit Modes:**
  - `contain` - Fit inside dimensions, maintain aspect ratio
  - `cover` - Fill entire area, crop if needed
  - `fill` - Stretch to fill, ignore aspect ratio

#### 2. **ImageRotator** - Rotation & Flipping
- **Methods:**
  - `rotate(input, degrees, options)` - Rotate by any angle (90°, 180°, 270°, etc.)
  - `flip(input, direction, options)` - Flip horizontally or vertically
  
- **Features:**
  - Precise rotation with automatic canvas sizing
  - Horizontal and vertical flipping
  - High-quality output

#### 3. **ImageCompressor** - Smart Compression
- **Methods:**
  - `compress(input, options)` - Compress to target file size
  - `optimize(input, options)` - Smart optimization
  
- **Features:**
  - Target file size control (maxSizeKB)
  - Automatic quality adjustment
  - Multiple format support (WebP, JPEG, PNG)
  - Smart compression with up to 10 attempts
  - Compression ratio reporting
  - Optional dimension limiting

#### 4. **TextOverlay** - Watermarks & Text
- **Methods:**
  - `addWatermark(input, options)` - Add watermark text
  - `addText(input, options)` - Add custom text overlay
  
- **Features:**
  - 5 position presets (top-left, top-right, bottom-left, bottom-right, center)
  - Custom positioning (x, y coordinates)
  - Font customization (family, size, color)
  - Text effects (shadow, stroke, background)
  - Opacity control
  - Bold and italic styles

#### 5. **ColorAdjuster** - Advanced Color Manipulation
- **Methods:**
  - `adjustColors(input, options)` - Adjust multiple color properties
  
- **Features:**
  - **Saturation** adjustment (0 to 2x)
  - **Hue** rotation (-180° to +180°)
  - **Lightness** adjustment (-50 to +50)
  - **Vibrance** control (0 to 2x)
  - HSL color space manipulation
  - Pixel-perfect color transformation

---

## 📊 Technical Details

### File Sizes
- **Source:** 118 KB (3,127 lines)
- **Minified:** 46 KB
- **Added:** ~966 lines of new code

### New Wrapper Methods in Main EIIP Class
```javascript
// Resize methods
await eiip.resizeImage(file, { width: 800, height: 600, fit: 'contain' })
await eiip.scaleImage(file, 0.5)
await eiip.createThumbnail(file, { size: 150 })

// Rotation methods
await eiip.rotateImage(file, 90)
await eiip.flipImage(file, 'horizontal')

// Compression methods
await eiip.compressImage(file, { maxSizeKB: 500, format: 'webp' })
await eiip.optimizeImage(file)

// Text overlay methods
await eiip.addWatermark(file, { text: '© 2024', position: 'bottom-right' })
await eiip.addText(file, { text: 'Hello', x: 'center', y: 'center' })

// Color adjustment methods
await eiip.adjustColors(file, { saturation: 1.5, hue: 30, lightness: 10 })
```

---

## 🎨 Updated Demo Page

The demo HTML (`eiip-demo.html`) now includes **5 new interactive sections**:

### 1. Image Resize & Scale
- Width/Height inputs
- Fit mode selector (contain/cover/fill)
- Scale by factor
- One-click thumbnail creation

### 2. Image Rotation & Flip
- Quick rotation buttons (90°, 180°, 270°)
- Horizontal & vertical flip buttons
- Live preview

### 3. Image Compression
- Target size control
- Format selection (WebP/JPEG/PNG)
- Smart optimize mode
- Compression ratio display

### 4. Text Overlay & Watermark
- Watermark with position presets
- Custom text with color picker
- Font and style options
- Shadow effects

### 5. Color Adjustment
- Real-time sliders for:
  - Saturation (0-2x)
  - Hue (-180° to +180°)
  - Lightness (-50 to +50)
  - Vibrance (0-2x)
- Reset button
- Live value display

---

## 💻 Usage Examples

### Example 1: Create Optimized Thumbnail
```javascript
const eiip = new EIIP();

// Create a 200x200 thumbnail with WebP compression
const result = await eiip.createThumbnail(imageFile, {
    size: 200,
    format: 'webp',
    quality: 0.85
});

console.log(`Thumbnail: ${result.width}x${result.height}`);
```

### Example 2: Add Watermark
```javascript
// Add semi-transparent watermark to bottom-right
const result = await eiip.addWatermark(imageFile, {
    text: '© 2024 Your Company',
    position: 'bottom-right',
    opacity: 0.7,
    color: '#ffffff',
    shadow: true
});
```

### Example 3: Smart Compression
```javascript
// Compress image to under 300KB
const result = await eiip.compressImage(imageFile, {
    maxSizeKB: 300,
    format: 'webp',
    maxWidth: 1920,
    maxHeight: 1080
});

console.log(`Compressed: ${result.sizeKB}KB (${result.compressionRatio}% smaller)`);
```

### Example 4: Color Grading
```javascript
// Apply cinematic color grading
const result = await eiip.adjustColors(imageFile, {
    saturation: 1.3,     // Increase saturation by 30%
    hue: 15,             // Warm color shift
    lightness: 5,        // Slight brightness boost
    vibrance: 1.2        // Enhance vibrance
});
```

### Example 5: Batch Processing
```javascript
// Resize, rotate, add watermark, and compress
const eiip = new EIIP();

// 1. Resize
let result = await eiip.resizeImage(imageFile, {
    width: 1200,
    height: 800,
    fit: 'cover'
});

// 2. Rotate
result = await eiip.rotateImage(result.canvas, 90);

// 3. Add watermark
result = await eiip.addWatermark(result.canvas, {
    text: '© 2024',
    position: 'bottom-right'
});

// 4. Compress
result = await eiip.compressImage(result.canvas, {
    maxSizeKB: 500,
    format: 'webp'
});

// Download final result
eiip.downloadImage(result.dataUrl, 'processed-image.webp');
```

---

## 🔧 API Constants

### ImageResizer.FIT_MODES
- `CONTAIN` - 'contain'
- `COVER` - 'cover'
- `FILL` - 'fill'

### ImageRotator.DIRECTIONS
- `HORIZONTAL` - 'horizontal'
- `VERTICAL` - 'vertical'

### TextOverlay.POSITIONS
- `TOP_LEFT` - 'top-left'
- `TOP_RIGHT` - 'top-right'
- `BOTTOM_LEFT` - 'bottom-left'
- `BOTTOM_RIGHT` - 'bottom-right'
- `CENTER` - 'center'

---

## 🚀 Performance

All operations are optimized for:
- ✅ High-quality output (using `imageSmoothingQuality = 'high'`)
- ✅ Fast processing with Canvas API
- ✅ Minimal memory footprint
- ✅ Support for large images
- ✅ Progressive quality adjustment (compression)

---

## 🌟 Complete Feature List (v1.2.0)

EIIP now includes **11 powerful classes**:

1. **SvgToRaster** - Convert SVG to raster images (PNG, JPG, WebP)
2. **RasterToSvg** - Convert raster to SVG with tracing
3. **ImageLayerMerger** - Merge multiple image layers
4. **ImageCropper** - Crop images (rectangle, circle, aspect ratios)
5. **ImageEffects** - Apply 10+ filters and effects
6. **PDFConverter** - Convert images to PDF documents
7. **ImageResizer** ⭐ NEW - Resize and scale images
8. **ImageRotator** ⭐ NEW - Rotate and flip images
9. **ImageCompressor** ⭐ NEW - Smart compression
10. **TextOverlay** ⭐ NEW - Add text and watermarks
11. **ColorAdjuster** ⭐ NEW - Advanced color manipulation

---

## 📦 Installation

```bash
npm install eiip
```

Or use CDN:
```html
<script src="https://unpkg.com/eiip@1.2.0/eiip.min.js"></script>
```

---

## 🔄 Migration from v1.1.0

All v1.1.0 features are **fully backward compatible**. New features are additive, so existing code will work without changes.

Simply update to v1.2.0:
```bash
npm update eiip
```

---

## 📚 Documentation

- **API Reference:** See `API-REFERENCE.md` (will be updated)
- **Examples:** See `EXAMPLES-NEW-FEATURES.md`
- **Demo:** Open `eiip-demo.html` in browser

---

## 🐛 Bug Fixes

No bug fixes in this release (v1.1.0 was stable).

---

## 🎯 What's Next?

Planned for **v1.3.0**:
- Background Removal
- Object Detection
- Image Comparison
- Color Palette Extraction
- Image Stitching/Panorama

Planned for **v1.4.0**:
- Format Conversion (HEIC, TIFF, etc.)
- EXIF Data Management
- Image Filters Pipeline
- Batch Processing Helper

---

## 👨‍💻 Author

**Saleem Ahmad** - Elite India  
📧 Contact: [Your Email]  
🌐 Website: [Your Website]

---

## 📄 License

MIT License - See LICENSE file for details

---

## ⭐ Support

If you find EIIP useful, please:
- ⭐ Star the repository
- 🐛 Report issues
- 💡 Suggest features
- 🤝 Contribute code

---

**Thank you for using EIIP!** 🎨✨
