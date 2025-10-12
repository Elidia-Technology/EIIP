# EIIP - Elite India Image Processing

[![npm version](https://badge.fury.io/js/eiip.svg)](https://badge.fury.io/js/eiip)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/SaleemLww/EIIP-Frontend.svg)](https://github.com/SaleemLww/EIIP-Frontend/stargazers)

A comprehensive **client-side** image processing library that delivers lightweight, efficient image processing directly in the browser. Reduce server load and provide instant results with powerful capabilities:

- 🎨 **SVG to Raster Conversion** - Convert SVG graphics to PNG, JPG, WebP with knitting effects
- 🖼️ **Raster to SVG Conversion** - Transform raster images to optimized SVG with color reduction
- 🔧 **Advanced Layer Merging** - Combine multiple image layers with positioning, scaling, and effects
- ✂️ **Image Cropping** - Crop to rectangle, circle, or aspect ratio (NEW!)
- 🌈 **Image Effects & Filters** - 10+ predefined effects: grayscale, sepia, vintage, and more (NEW!)
- 📄 **PDF Conversion** - Convert images to PDF directly in the browser (NEW!)

**Client-first design** with zero server dependencies. Compatible with **React**, **Angular**, **Vue.js**, and **vanilla JavaScript**. Also works in Node.js environments for versatile deployment options.

## ✨ Features

### SVG to Raster Converter
- Convert SVG to PNG, JPG, JPEG, WebP formats
- Flexible sizing with aspect ratio preservation
- Advanced knitting effects and yarn textures
- Multiple input methods (File, URL, String, DOM Element)
- High-performance canvas-based rendering
- Quality and compression controls

### Raster to SVG Converter
- Convert PNG, JPG, GIF, BMP, WebP to SVG
- Intelligent color reduction and optimization
- Configurable color palettes (2-16M colors)
- Pixel-perfect conversion with customizable scaling
- Color preset options for different use cases

### Image Layer Merger
- Merge multiple image layers into single output
- Support for PNG, JPEG, SVG images
- Advanced positioning (absolute, center, relative)
- Layer effects: opacity, rotation, scaling, z-index
- DPI scaling for print-quality output
- Background color and transparency support

### Image Cropper (NEW!)
- Crop to specific rectangle area
- Crop to perfect circle (ideal for avatars)
- Crop to aspect ratio (Square, 16:9, 4:3, Instagram, Facebook cover, etc.)
- Maintain image quality during cropping
- Multiple input formats supported

### Image Effects & Filters (NEW!)
- **10+ Built-in Effects**: Grayscale, Sepia, Vintage, Blur, Sharpen, Invert
- **Color Adjustments**: Brightness, Contrast, Warm, Cool
- **Intensity Control**: Fine-tune effect strength (0.0 - 1.0)
- **Chain Effects**: Apply multiple effects in sequence
- **Real-time Processing**: Instant preview in browser

### PDF Converter (NEW!)
- Convert single or multiple images to PDF
- Multiple page sizes: A4, Letter, Legal, A3, A5, Tabloid
- Portrait and landscape orientations
- Customizable margins
- Image fitting options: contain, cover, fill
- Client-side PDF generation (no server required)

## 🏎️ Client-Side Processing Power

**EIIP is designed for efficient client-side image processing** - reducing server load and providing instant results to your users:

### ✅ Why Client-Side?
- **Zero Server Load** - All processing happens in the user's browser
- **Instant Processing** - No network delays or server queues
- **Privacy First** - Images never leave the user's device
- **Lightweight** - Optimized JavaScript with minimal bundle size
- **Offline Capable** - Works without internet connection
- **Scalable** - Automatically scales with your user base

### 🖥️ Browser Performance
- **Modern Canvas API** - Hardware-accelerated rendering
- **Web Workers Ready** - Can be used in background threads
- **Memory Efficient** - Smart garbage collection and cleanup
- **Cross-Platform** - Works on desktop, mobile, and tablets

### 🔧 Server-Side Alternative
While EIIP excels at client-side processing, for advanced server-side image processing with more sophisticated features, consider **[EIGC (Elite India Graphics Core)](https://github.com/SaleemLww/EIGC)** - our comprehensive server-side image processing library with enterprise-grade capabilities.

## 📦 Installation

### NPM Installation

```bash
npm install eiip
```

```bash
yarn add eiip
```

### CDN Usage

```html
<!-- Full version -->
<script src="https://unpkg.com/eiip@latest/eiip.js"></script>

<!-- Minified version -->
<script src="https://unpkg.com/eiip@latest/eiip.min.js"></script>
```

### Direct Download

Download `eiip.js` or `eiip.min.js` and include in your project:

```html
<script src="path/to/eiip.min.js"></script>
```

## 🚀 Quick Start

### Basic Usage

```javascript
// Initialize EIIP
const eiip = new EIIP({
    debug: true // Enable console logging
});

// Convert SVG to PNG
const svgResult = await eiip.convertSvgToRaster(svgString, {
    width: 800,
    height: 600,
    format: 'png',
    quality: 0.9
});

// Convert PNG to SVG
const rasterResult = await eiip.convertRasterToSvg(imageFile, {
    colorReductionLevel: 16, // 4,096 colors
    scaleFactor: 0.5
});

// Merge multiple image layers
const mergedResult = await eiip.mergeLayers([
    { src: 'background.png', x: 0, y: 0, zIndex: 1 },
    { src: 'logo.svg', x: 'center', y: 'center', scale: 0.5, zIndex: 2 }
], {
    format: 'png',
    quality: 0.9
});

// NEW: Crop image to circle (perfect for avatars)
const circularImage = await eiip.cropToCircle(imageFile, {
    size: 500,
    format: 'png'
});

// NEW: Apply vintage effect
const vintageImage = await eiip.applyEffect(imageFile, 'vintage', 0.8, {
    format: 'jpg',
    quality: 0.9
});

// NEW: Convert images to PDF
const pdf = await eiip.convertImagesToPDF([image1, image2, image3], {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.A4,
    orientation: 'portrait'
});
```

### New Features Examples

See **[EXAMPLES-NEW-FEATURES.md](./EXAMPLES-NEW-FEATURES.md)** for comprehensive examples of:
- Image cropping (rectangle, circle, aspect ratio)
- Applying effects and filters
- PDF conversion
- Combined workflows

## 📚 Framework Integration

### React Example

```jsx
import React, { useState, useRef } from 'react';
import EIIP from 'eiip';

const ImageProcessor = () => {
    const [result, setResult] = useState(null);
    const [processing, setProcessing] = useState(false);
    const fileInputRef = useRef(null);

    const handleSvgToRaster = async (file) => {
        setProcessing(true);
        try {
            const eiip = new EIIP();
            const result = await eiip.convertSvgToRaster(file, {
                width: 1000,
                height: 800,
                format: 'png',
                quality: 0.95
            });
            setResult(result.dataUrl);
        } catch (error) {
            console.error('Processing failed:', error);
        } finally {
            setProcessing(false);
        }
    };

    const handleLayerMerging = async () => {
        setProcessing(true);
        try {
            const eiip = new EIIP();
            const result = await eiip.mergeLayers([
                { src: '/background.jpg', x: 0, y: 0, width: 800, height: 600 },
                { src: '/overlay.png', x: 'center', y: 'center', opacity: 0.7 }
            ]);
            setResult(result.dataUrl);
        } catch (error) {
            console.error('Merging failed:', error);
        } finally {
            setProcessing(false);
        }
    };

    return (
        <div className="image-processor">
            <input
                ref={fileInputRef}
                type="file"
                accept=".svg"
                onChange={(e) => handleSvgToRaster(e.target.files[0])}
            />
            
            <button onClick={handleLayerMerging} disabled={processing}>
                {processing ? 'Processing...' : 'Merge Layers'}
            </button>

            {result && (
                <div className="result">
                    <img src={result} alt="Processed" style={{ maxWidth: '100%' }} />
                    <button onClick={() => {
                        const eiip = new EIIP();
                        eiip.downloadImage(result, 'processed-image.png');
                    }}>
                        Download
                    </button>
                </div>
            )}
        </div>
    );
};

export default ImageProcessor;
```

### Angular Example

```typescript
import { Component } from '@angular/core';
import EIIP from 'eiip';

@Component({
  selector: 'app-image-processor',
  template: `
    <div class="image-processor">
      <input type="file" (change)="onFileSelect($event)" accept=".png,.jpg,.svg">
      <button (click)="processImage()" [disabled]="processing">
        {{ processing ? 'Processing...' : 'Process Image' }}
      </button>
      <img *ngIf="result" [src]="result" alt="Processed" style="max-width: 100%">
    </div>
  `
})
export class ImageProcessorComponent {
  selectedFile: File | null = null;
  result: string | null = null;
  processing = false;

  onFileSelect(event: any) {
    this.selectedFile = event.target.files[0];
  }

  async processImage() {
    if (!this.selectedFile) return;

    this.processing = true;
    try {
      const eiip = new EIIP();
      
      if (this.selectedFile.type.includes('svg')) {
        const result = await eiip.convertSvgToRaster(this.selectedFile, {
          format: 'png',
          quality: 0.9
        });
        this.result = result.dataUrl;
      } else {
        const result = await eiip.convertRasterToSvg(this.selectedFile, {
          colorReductionLevel: EIIP.RasterToSvg.COLOR_PRESETS.MEDIUM
        });
        // Convert SVG back to data URL for display
        const blob = new Blob([result.svg], { type: 'image/svg+xml' });
        this.result = URL.createObjectURL(blob);
      }
    } catch (error) {
      console.error('Processing failed:', error);
    } finally {
      this.processing = false;
    }
  }
}
```

### Vue.js Example

```vue
<template>
  <div class="image-processor">
    <input 
      type="file" 
      @change="handleFileSelect" 
      accept=".svg,.png,.jpg"
      ref="fileInput"
    />
    
    <button @click="convertToRaster" :disabled="processing">
      {{ processing ? 'Converting...' : 'Convert to Raster' }}
    </button>
    
    <button @click="mergeLayers" :disabled="processing">
      {{ processing ? 'Merging...' : 'Merge Layers' }}
    </button>

    <div v-if="result" class="result">
      <img :src="result" alt="Processed" style="max-width: 100%" />
      <button @click="downloadResult">Download</button>
    </div>
  </div>
</template>

<script>
import EIIP from 'eiip';

export default {
  name: 'ImageProcessor',
  data() {
    return {
      selectedFile: null,
      result: null,
      processing: false,
      eiip: new EIIP({ debug: true })
    };
  },
  methods: {
    handleFileSelect(event) {
      this.selectedFile = event.target.files[0];
    },

    async convertToRaster() {
      if (!this.selectedFile) return;

      this.processing = true;
      try {
        const result = await this.eiip.convertSvgToRaster(this.selectedFile, {
          width: 800,
          height: 600,
          format: 'png'
        });
        this.result = result.dataUrl;
      } catch (error) {
        console.error('Conversion failed:', error);
      } finally {
        this.processing = false;
      }
    },

    async mergeLayers() {
      this.processing = true;
      try {
        const result = await this.eiip.mergeLayers([
          { src: '/assets/bg.jpg', x: 0, y: 0 },
          { src: '/assets/logo.png', x: 'center', y: 'center', scale: 0.3 }
        ]);
        this.result = result.dataUrl;
      } catch (error) {
        console.error('Merging failed:', error);
      } finally {
        this.processing = false;
      }
    },

    downloadResult() {
      if (this.result) {
        this.eiip.downloadImage(this.result, 'processed-image.png');
      }
    }
  }
};
</script>
```

## 📖 API Reference

For complete API documentation, see **[API-REFERENCE.md](./API-REFERENCE.md)**

### Quick Reference

**Main EIIP Class:**
```javascript
const eiip = new EIIP(options);
```

**Key Methods:**
- `convertSvgToRaster(input, options)` - Convert SVG to PNG/JPG/WebP
- `convertRasterToSvg(input, options)` - Convert images to SVG
- `mergeLayers(layers, options)` - Merge multiple image layers
- `downloadImage(dataUrl, filename)` - Download processed images

**Direct Class Access:**
- `EIIP.SvgToRaster` - SVG conversion class
- `EIIP.RasterToSvg` - Raster conversion class
- `EIIP.ImageLayerMerger` - Layer merging class

**Complete documentation with all options, examples, and advanced usage is available in [API-REFERENCE.md](./API-REFERENCE.md)**

##  License

MIT License - Copyright (c) 2025 Saleem Ahmad (Elite India)

## 👨‍💻 Author

**Saleem Ahmad (Elite India)**
- GitHub: [@SaleemLww](https://github.com/SaleemLww)
- Repository: [EIIP-Frontend](https://github.com/SaleemLww/EIIP-Frontend)

---

**EIIP** - Elite India Image Processing Library - Making frontend image processing simple and powerful! 🎨✨