# EIIP - Elite India Image Processing

[![npm version](https://badge.fury.io/js/eiip.svg)](https://badge.fury.io/js/eiip)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub stars](https://img.shields.io/github/stars/SaleemLww/EIIP-Frontend.svg)](https://github.com/SaleemLww/EIIP-Frontend/stargazers)

A comprehensive frontend image processing library that combines three powerful image processing capabilities:

- 🎨 **SVG to Raster Conversion** - Convert SVG graphics to PNG, JPG, WebP with knitting effects
- 🖼️ **Raster to SVG Conversion** - Transform raster images to optimized SVG with color reduction
- 🔧 **Advanced Layer Merging** - Combine multiple image layers with positioning, scaling, and effects

Compatible with **React**, **Angular**, **Vue.js**, and **vanilla JavaScript**. Works in both browser and Node.js environments.

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
```

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

### EIIP Main Class

```javascript
const eiip = new EIIP(options);
```

**Options:**
- `debug: boolean` - Enable debug logging (default: false)
- `width: number` - Default output width
- `height: number` - Default output height
- `format: string` - Default output format ('png', 'jpg', 'webp')
- `quality: number` - Default quality (0.0-1.0)

### Methods

#### `convertSvgToRaster(input, options)`
#### `convertRasterToSvg(input, options)`
#### `mergeLayers(layers, options)`
#### `downloadImage(dataUrl, filename)`

### Static Classes
- `EIIP.SvgToRaster` - Direct access to SVG converter
- `EIIP.RasterToSvg` - Direct access to Raster converter  
- `EIIP.ImageLayerMerger` - Direct access to Layer merger

## 🔧 NPM Publishing

To publish this library to NPM:

```bash
# Build the minified version
npm run build

# Login to NPM (if not already logged in)
npm login

# Publish to NPM
npm publish
```

## 📄 License

MIT License - Copyright (c) 2025 Saleem Ahmad (Elite India)

## 👨‍💻 Author

**Saleem Ahmad (Elite India)**
- GitHub: [@SaleemLww](https://github.com/SaleemLww)
- Repository: [EIIP-Frontend](https://github.com/SaleemLww/EIIP-Frontend)

---

**EIIP** - Elite India Image Processing Library - Making frontend image processing simple and powerful! 🎨✨