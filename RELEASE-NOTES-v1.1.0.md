# EIIP v1.1.0 Release Notes

## 🎉 Major Update: New Features Added!

We're excited to announce EIIP v1.1.0 with three powerful new capabilities for client-side image processing!

---

## ✨ What's New

### 1. ✂️ Image Cropper
Professional image cropping with multiple modes:
- **Rectangle Crop** - Crop to specific coordinates and dimensions
- **Circle Crop** - Perfect for profile pictures and avatars
- **Aspect Ratio Crop** - 7 preset ratios including 16:9, Instagram, Facebook cover, and more
- **Custom Ratios** - Define your own aspect ratios

**Quick Example:**
```javascript
// Crop to circle
const result = await eiip.cropToCircle(imageFile, {
    size: 500,
    format: 'png'
});

// Crop to 16:9 aspect ratio
const widescreen = await eiip.cropToAspectRatio(
    imageFile,
    EIIP.ImageCropper.ASPECT_RATIOS.LANDSCAPE
);
```

### 2. 🌈 Image Effects & Filters
10+ professional-grade effects with intensity control:
- **Grayscale** - Classic black and white
- **Sepia** - Warm vintage tone
- **Vintage** - Complete retro look with vignette
- **Brightness** - Adjust image brightness
- **Contrast** - Enhance or reduce contrast
- **Warm** - Add warm tones (more red/orange)
- **Cool** - Add cool tones (more blue)
- **Blur** - Soften image
- **Sharpen** - Enhance details
- **Invert** - Negative effect

**Quick Example:**
```javascript
// Apply single effect
const vintage = await eiip.applyEffect(imageFile, 'vintage', 0.8);

// Chain multiple effects
const result = await eiip.applyEffects(imageFile, [
    { name: 'brightness', intensity: 1.2 },
    { name: 'contrast', intensity: 1.1 },
    { name: 'warm', intensity: 0.3 }
]);
```

### 3. 📄 PDF Converter
Convert images to PDF directly in the browser:
- **Multiple Page Sizes** - A4, Letter, Legal, A3, A5, Tabloid
- **Orientations** - Portrait and landscape
- **Custom Margins** - Control page margins
- **Fitting Options** - Contain, cover, or fill
- **Multi-page Support** - Convert multiple images to one PDF

**Quick Example:**
```javascript
// Single image to PDF
const pdf = await eiip.convertImageToPDF(imageFile, {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.A4,
    orientation: 'portrait'
});

// Multiple images to PDF
const multiPagePDF = await eiip.convertImagesToPDF([img1, img2, img3], {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.LETTER,
    orientation: 'landscape'
});
```

---

## 🚀 API Additions

### New Methods on Main EIIP Class

```javascript
const eiip = new EIIP();

// Cropping methods
await eiip.cropImage(input, cropArea, options)
await eiip.cropToCircle(input, options)
await eiip.cropToAspectRatio(input, aspectRatio, options)

// Effects methods
await eiip.applyEffect(input, effectName, intensity, options)
await eiip.applyEffects(input, effects, options)

// PDF conversion methods
await eiip.convertImageToPDF(input, options)
await eiip.convertImagesToPDF(inputs, options)
```

### New Static Classes

```javascript
// Direct access to new classes
EIIP.ImageCropper
EIIP.ImageEffects
EIIP.PDFConverter

// Use them independently
const cropper = new EIIP.ImageCropper({ debug: true });
const result = await cropper.cropToCircle(image, { size: 400 });
```

---

## 📦 What's Included

### New Files in Package
- **EXAMPLES-NEW-FEATURES.md** - Comprehensive examples for all new features
- **new-features-demo.html** - Interactive demo page
- Updated **README.md** with new feature highlights
- Updated **API-REFERENCE.md** with complete documentation

### Constants & Presets

**Image Cropper:**
```javascript
EIIP.ImageCropper.ASPECT_RATIOS = {
    SQUARE: { width: 1, height: 1 },
    LANDSCAPE: { width: 16, height: 9 },
    PORTRAIT: { width: 9, height: 16 },
    CLASSIC: { width: 4, height: 3 },
    WIDE: { width: 21, height: 9 },
    INSTAGRAM: { width: 4, height: 5 },
    FACEBOOK_COVER: { width: 820, height: 312 }
}
```

**Image Effects:**
```javascript
EIIP.ImageEffects.EFFECT_NAMES = [
    'GRAYSCALE', 'SEPIA', 'BRIGHTNESS', 'CONTRAST',
    'INVERT', 'BLUR', 'SHARPEN', 'VINTAGE', 'WARM', 'COOL'
]
```

**PDF Converter:**
```javascript
EIIP.PDFConverter.PAGE_SIZES = {
    A4: { width: 595, height: 842 },
    LETTER: { width: 612, height: 792 },
    LEGAL: { width: 612, height: 1008 },
    A3: { width: 842, height: 1191 },
    A5: { width: 420, height: 595 },
    TABLOID: { width: 792, height: 1224 }
}
```

---

## 🎯 Use Cases

### Social Media Content Creation
```javascript
// Instagram post (1:1)
const post = await eiip.cropToAspectRatio(
    image,
    EIIP.ImageCropper.ASPECT_RATIOS.SQUARE
);

// Apply vintage filter
const styled = await eiip.applyEffect(post.canvas, 'vintage', 0.7);
```

### Profile Picture Generation
```javascript
// Crop to circle and enhance
const squared = await eiip.cropToAspectRatio(
    uploadedImage,
    EIIP.ImageCropper.ASPECT_RATIOS.SQUARE
);
const circular = await eiip.cropToCircle(squared.canvas, { size: 500 });
const enhanced = await eiip.applyEffect(circular.dataUrl, 'warm', 0.2);
```

### Document Creation
```javascript
// Convert photos to PDF document
const photos = [photo1, photo2, photo3, photo4];
const document = await eiip.convertImagesToPDF(photos, {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.A4,
    orientation: 'portrait',
    margin: 30
});
eiip.downloadImage(document.downloadUrl, 'photo-album.pdf');
```

---

## 🔄 Migration Guide

### From v1.0.x to v1.1.0

**No Breaking Changes!** All existing code continues to work.

Simply install the new version:
```bash
npm install eiip@latest
```

Or update via CDN:
```html
<script src="https://unpkg.com/eiip@1.1.0/eiip.min.js"></script>
```

---

## 📊 Performance

All new features are:
- ✅ **Client-side only** - No server dependencies
- ✅ **Lightweight** - Minimal bundle size increase
- ✅ **Fast** - Hardware-accelerated canvas operations
- ✅ **Memory efficient** - Proper cleanup and garbage collection
- ✅ **Browser compatible** - Works on Chrome 60+, Firefox 55+, Safari 11+, Edge 79+

---

## 🎨 Framework Support

All new features work seamlessly with:
- React
- Vue.js
- Angular
- Vanilla JavaScript
- Any framework that supports ES6/CommonJS

---

## 📚 Documentation

- **README.md** - Overview and quick start
- **API-REFERENCE.md** - Complete API documentation
- **EXAMPLES-NEW-FEATURES.md** - Comprehensive examples
- **new-features-demo.html** - Interactive demo

---

## 🛣️ Roadmap (v1.2.0 and beyond)

Planned features for future releases:
- Advanced filters (Gaussian blur, edge detection, etc.)
- Image resize and scale utilities
- Batch processing optimizations
- WebP optimization
- Image compression utilities
- Color palette extraction
- Text overlay on images
- QR code generation
- Barcode scanning (with external library integration)

---

## 💬 Feedback & Contributions

We'd love to hear from you!
- **Issues**: [GitHub Issues](https://github.com/SaleemLww/EIIP-Frontend/issues)
- **Discussions**: [GitHub Discussions](https://github.com/SaleemLww/EIIP-Frontend/discussions)
- **NPM**: [npmjs.com/package/eiip](https://www.npmjs.com/package/eiip)

---

## 📄 License

MIT License - Copyright (c) 2025 Saleem Ahmad (Elite India)

---

## 🙏 Thank You!

Thank you for using EIIP! We're committed to making client-side image processing simple, powerful, and accessible to everyone.

**Happy Coding!** 🎨✨

---

**EIIP v1.1.0** - Making frontend image processing even more powerful!
