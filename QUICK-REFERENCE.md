# EIIP v1.2.0 Quick Reference Guide

## 🚀 Quick Start

```bash
npm install eiip
```

```javascript
import EIIP from 'eiip';
const eiip = new EIIP({ debug: true });
```

---

## 📏 Image Resize & Scale

### Resize to Dimensions
```javascript
const result = await eiip.resizeImage(file, {
    width: 800,
    height: 600,
    fit: 'contain',  // or 'cover', 'fill'
    format: 'png',
    quality: 0.92
});
```

### Scale by Factor
```javascript
const result = await eiip.scaleImage(file, 0.5, {
    format: 'png',
    quality: 0.92
});
```

### Create Thumbnail
```javascript
const result = await eiip.createThumbnail(file, {
    size: 150,
    format: 'webp',
    quality: 0.85
});
```

**Fit Modes:**
- `contain` - Fit inside, maintain aspect ratio
- `cover` - Fill area, crop if needed
- `fill` - Stretch to fill

---

## 🔄 Image Rotation & Flip

### Rotate Image
```javascript
// Rotate 90 degrees
const result = await eiip.rotateImage(file, 90, {
    format: 'png',
    quality: 0.92
});

// Common angles: 90, 180, 270
```

### Flip Image
```javascript
// Flip horizontal
const result = await eiip.flipImage(file, 'horizontal', {
    format: 'png'
});

// Or vertical
const result = await eiip.flipImage(file, 'vertical');
```

---

## 🗜️ Image Compression

### Compress to Target Size
```javascript
const result = await eiip.compressImage(file, {
    maxSizeKB: 500,
    format: 'webp',
    quality: 0.9,
    maxWidth: 1920,
    maxHeight: 1080
});

console.log(`Compressed to ${result.sizeKB}KB`);
```

### Smart Optimize
```javascript
const result = await eiip.optimizeImage(file, {
    maxSizeKB: 800,
    maxWidth: 1920,
    maxHeight: 1080,
    format: 'webp',
    quality: 0.85
});
```

**Best Formats:**
- `webp` - Best compression (recommended)
- `jpeg` - Good for photos
- `png` - Lossless quality

---

## 💬 Text Overlay & Watermark

### Add Watermark
```javascript
const result = await eiip.addWatermark(file, {
    text: '© 2024 Your Company',
    position: 'bottom-right',
    opacity: 0.7,
    color: '#ffffff',
    shadow: true,
    fontSize: 24,
    fontFamily: 'Arial'
});
```

**Positions:**
- `top-left`, `top-right`
- `bottom-left`, `bottom-right`
- `center`

### Add Custom Text
```javascript
const result = await eiip.addText(file, {
    text: 'Hello World',
    fontSize: 60,
    color: '#000000',
    x: 'center',  // or pixel value
    y: 'center',  // or pixel value
    bold: true,
    italic: false,
    shadow: true,
    shadowColor: 'rgba(0,0,0,0.7)',
    shadowBlur: 4,
    stroke: true,
    strokeColor: '#ffffff',
    strokeWidth: 2,
    backgroundColor: '#ffffff'
});
```

---

## 🎨 Color Adjustment

### Adjust Colors
```javascript
const result = await eiip.adjustColors(file, {
    saturation: 1.5,    // 0 to 2 (1 = no change)
    hue: 30,            // -180 to 180 degrees
    lightness: 10,      // -50 to 50
    vibrance: 1.2,      // 0 to 2 (1 = no change)
    format: 'png',
    quality: 0.92
});
```

**Parameters:**
- **Saturation**: Color intensity (0 = grayscale, 2 = vibrant)
- **Hue**: Color shift (-180° to +180°)
- **Lightness**: Brightness (-50 = darker, +50 = lighter)
- **Vibrance**: Selective saturation (0 = muted, 2 = vivid)

---

## 🔗 Chaining Operations

```javascript
const eiip = new EIIP();

// 1. Resize
let result = await eiip.resizeImage(file, {
    width: 1200,
    height: 800,
    fit: 'cover'
});

// 2. Rotate
result = await eiip.rotateImage(result.canvas, 90);

// 3. Adjust colors
result = await eiip.adjustColors(result.canvas, {
    saturation: 1.3,
    hue: 15
});

// 4. Add watermark
result = await eiip.addWatermark(result.canvas, {
    text: '© 2024',
    position: 'bottom-right'
});

// 5. Compress
result = await eiip.compressImage(result.canvas, {
    maxSizeKB: 500,
    format: 'webp'
});

// Download
eiip.downloadImage(result.dataUrl, 'final.webp');
```

---

## 📊 Result Object

All methods return a result object with:

```javascript
{
    canvas: HTMLCanvasElement,  // Canvas element
    dataUrl: string,            // Data URL
    blob: Blob,                 // Blob object
    width: number,              // Output width
    height: number,             // Output height
    // Method-specific properties...
}
```

---

## 🎯 Common Use Cases

### Profile Picture Processing
```javascript
// Resize, crop to circle, add watermark
const resized = await eiip.resizeImage(file, {
    width: 400,
    height: 400,
    fit: 'cover'
});

const circular = await eiip.cropToCircle(resized.canvas);

const final = await eiip.addWatermark(circular.canvas, {
    text: '©',
    position: 'bottom-right',
    fontSize: 16
});
```

### Product Photo Optimization
```javascript
// Resize, enhance colors, compress
const resized = await eiip.resizeImage(file, {
    width: 1200,
    height: 1200,
    fit: 'contain'
});

const enhanced = await eiip.adjustColors(resized.canvas, {
    saturation: 1.2,
    lightness: 5,
    vibrance: 1.1
});

const optimized = await eiip.compressImage(enhanced.canvas, {
    maxSizeKB: 300,
    format: 'webp'
});
```

### Thumbnail Generation
```javascript
// Create multiple sizes
const thumb150 = await eiip.createThumbnail(file, { size: 150 });
const thumb300 = await eiip.createThumbnail(file, { size: 300 });
const thumb600 = await eiip.createThumbnail(file, { size: 600 });
```

### Bulk Watermarking
```javascript
const files = [...]; // array of files

for (const file of files) {
    const result = await eiip.addWatermark(file, {
        text: '© 2024',
        position: 'bottom-right'
    });
    
    eiip.downloadImage(result.dataUrl, `watermarked-${file.name}`);
}
```

---

## 🌐 Framework Integration

### React
```jsx
import EIIP from 'eiip';

function ImageEditor() {
    const eiip = new EIIP();
    
    const handleResize = async (file) => {
        const result = await eiip.resizeImage(file, {
            width: 800,
            height: 600,
            fit: 'contain'
        });
        
        setImageUrl(result.dataUrl);
    };
    
    return <input type="file" onChange={(e) => handleResize(e.target.files[0])} />;
}
```

### Vue.js
```vue
<script setup>
import EIIP from 'eiip';

const eiip = new EIIP();

async function resizeImage(file) {
    const result = await eiip.resizeImage(file, {
        width: 800,
        height: 600
    });
    
    imageUrl.value = result.dataUrl;
}
</script>
```

### Angular
```typescript
import EIIP from 'eiip';

@Component({...})
export class ImageEditorComponent {
    private eiip = new EIIP();
    
    async resizeImage(file: File) {
        const result = await this.eiip.resizeImage(file, {
            width: 800,
            height: 600,
            fit: 'contain'
        });
        
        this.imageUrl = result.dataUrl;
    }
}
```

---

## 🛠️ All Available Methods

### Resize & Scale
- `resizeImage(input, options)`
- `scaleImage(input, scaleFactor, options)`
- `createThumbnail(input, options)`

### Rotation & Flip
- `rotateImage(input, degrees, options)`
- `flipImage(input, direction, options)`

### Compression
- `compressImage(input, options)`
- `optimizeImage(input, options)`

### Text & Watermark
- `addWatermark(input, options)`
- `addText(input, options)`

### Color Adjustment
- `adjustColors(input, options)`

### Cropping (v1.1.0)
- `cropImage(input, cropArea, options)`
- `cropToCircle(input, options)`
- `cropToAspectRatio(input, aspectRatio, options)`

### Effects (v1.1.0)
- `applyEffect(input, effectName, intensity, options)`
- `applyEffects(input, effects, options)`

### PDF (v1.1.0)
- `convertImageToPDF(input, options)`
- `convertImagesToPDF(inputs, options)`

### Legacy
- `convertSvgToRaster(input, options)`
- `convertRasterToSvg(input, options)`
- `mergeLayers(layers, options)`

---

## 📚 Documentation

- **Full API:** `API-REFERENCE.md`
- **Examples:** `EXAMPLES-NEW-FEATURES.md`
- **Release Notes:** `RELEASE-NOTES-v1.2.0.md`
- **Demo:** `eiip-demo.html`

---

## 🎯 Tips & Best Practices

1. **Use WebP for compression** - Best quality/size ratio
2. **Chain operations efficiently** - Pass canvas between operations
3. **Set appropriate quality** - 0.85-0.92 for most cases
4. **Enable debug mode** - `new EIIP({ debug: true })` for development
5. **Handle errors** - Always use try/catch
6. **Optimize images** - Use `optimizeImage()` for best results
7. **Test with different formats** - PNG for quality, WebP for size, JPEG for photos

---

**Happy coding with EIIP!** 🎨✨
