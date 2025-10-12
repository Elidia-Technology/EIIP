# EIIP API Reference

Complete API documentation for EIIP (Elite India Image Processing) library.

**Version:** 1.2.0  
**Author:** Saleem Ahmad (Elite India)  
**License:** MIT

---

## Table of Contents

- [Installation](#installation)
- [Quick Start](#quick-start)
- [Main EIIP Class](#main-eiip-class)
  - [Constructor](#constructor)
  - [Methods](#methods)
  - [Static Properties](#static-properties)
- [SVG to Raster Conversion](#svg-to-raster-conversion)
  - [SvgToRaster Class](#svgtoraster-class)
  - [Conversion Options](#svg-conversion-options)
  - [Knitting Effects](#knitting-effects)
- [Raster to SVG Conversion](#raster-to-svg-conversion)
  - [RasterToSvg Class](#rastertosvg-class)
  - [Conversion Options](#raster-conversion-options)
  - [Color Reduction](#color-reduction)
- [Image Layer Merging](#image-layer-merging)
  - [ImageLayerMerger Class](#imagelayermerger-class)
  - [Layer Options](#layer-options)
  - [Merge Options](#merge-options)
- [Image Cropping](#image-cropping)
  - [ImageCropper Class](#imagecropper-class)
- [Image Effects & Filters](#image-effects--filters)
  - [ImageEffects Class](#imageeffects-class)
- [PDF Conversion](#pdf-conversion)
  - [PDFConverter Class](#pdfconverter-class)
- [Image Resize & Scale (v1.2.0)](#image-resize--scale-v120)
  - [ImageResizer Class](#imageresizer-class)
- [Image Rotation & Flip (v1.2.0)](#image-rotation--flip-v120)
  - [ImageRotator Class](#imagerotator-class)
- [Image Compression (v1.2.0)](#image-compression-v120)
  - [ImageCompressor Class](#imagecompressor-class)
- [Text Overlay & Watermark (v1.2.0)](#text-overlay--watermark-v120)
  - [TextOverlay Class](#textoverlay-class)
- [Color Adjustment (v1.2.0)](#color-adjustment-v120)
  - [ColorAdjuster Class](#coloradjuster-class)
- [Utility Methods](#utility-methods)
- [Error Handling](#error-handling)
- [Browser Compatibility](#browser-compatibility)

---

## Installation

### NPM
```bash
npm install eiip
```

### Yarn
```bash
yarn add eiip
```

### CDN
```html
<!-- Minified version (recommended) -->
<script src="https://unpkg.com/eiip@latest/eiip.min.js"></script>

<!-- Full version -->
<script src="https://unpkg.com/eiip@latest/eiip.js"></script>
```

---

## Quick Start

```javascript
// Import EIIP
import EIIP from 'eiip';
// or
const EIIP = require('eiip');
// or via CDN - EIIP is available globally

// Initialize
const eiip = new EIIP({ debug: true });

// Convert SVG to PNG
const result = await eiip.convertSvgToRaster(svgFile, {
    width: 800,
    height: 600,
    format: 'png',
    quality: 0.9
});

// Use the result
document.getElementById('output').src = result.dataUrl;
```

---

## Main EIIP Class

The main class that provides unified access to all image processing features.

### Constructor

```javascript
const eiip = new EIIP(options);
```

#### Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `debug` | `boolean` | `false` | Enable debug logging to console |
| `width` | `number` | `null` | Default output width (pixels) |
| `height` | `number` | `null` | Default output height (pixels) |
| `format` | `string` | `'png'` | Default output format: `'png'`, `'jpg'`, `'jpeg'`, `'webp'` |
| `quality` | `number` | `0.92` | Default quality (0.0 - 1.0) |

#### Example

```javascript
const eiip = new EIIP({
    debug: true,
    width: 1920,
    height: 1080,
    format: 'png',
    quality: 0.95
});
```

---

### Methods

#### `convertSvgToRaster(input, options)`

Convert SVG to raster format (PNG, JPG, WebP).

**Parameters:**
- `input` - SVG source (File, URL string, SVG string, or SVGElement)
- `options` - Conversion options (see [SVG Conversion Options](#svg-conversion-options))

**Returns:** `Promise<Object>`
```javascript
{
    canvas: HTMLCanvasElement,
    dataUrl: string,
    blob: Blob,
    width: number,
    height: number,
    format: string
}
```

**Example:**
```javascript
// From file
const fileResult = await eiip.convertSvgToRaster(svgFile, {
    width: 800,
    height: 600,
    format: 'png',
    quality: 0.9
});

// From URL
const urlResult = await eiip.convertSvgToRaster('https://example.com/image.svg', {
    width: 1000
});

// From SVG string
const svgString = '<svg width="100" height="100">...</svg>';
const stringResult = await eiip.convertSvgToRaster(svgString);

// From SVG DOM element
const svgElement = document.querySelector('svg');
const elementResult = await eiip.convertSvgToRaster(svgElement);
```

---

#### `convertRasterToSvg(input, options)`

Convert raster images (PNG, JPG, GIF, BMP, WebP) to SVG format.

**Parameters:**
- `input` - Image source (File, URL string, data URL, or HTMLImageElement)
- `options` - Conversion options (see [Raster Conversion Options](#raster-conversion-options))

**Returns:** `Promise<Object>`
```javascript
{
    svgString: string,
    svgDataUrl: string,
    blob: Blob,
    width: number,
    height: number,
    colorCount: number
}
```

**Example:**
```javascript
// From file
const fileResult = await eiip.convertRasterToSvg(imageFile, {
    colorReductionLevel: 16,
    scaleFactor: 0.5
});

// From URL
const urlResult = await eiip.convertRasterToSvg('https://example.com/image.png', {
    exactColorCount: 256
});

// From data URL
const dataUrlResult = await eiip.convertRasterToSvg(imageDataUrl);

// From image element
const imgElement = document.querySelector('img');
const elementResult = await eiip.convertRasterToSvg(imgElement);
```

---

#### `mergeLayers(layers, options)`

Merge multiple image layers into a single output image.

**Parameters:**
- `layers` - Array of layer objects (see [Layer Options](#layer-options))
- `options` - Merge options (see [Merge Options](#merge-options))

**Returns:** `Promise<Object>`
```javascript
{
    canvas: HTMLCanvasElement,
    dataUrl: string,
    blob: Blob,
    width: number,
    height: number
}
```

**Example:**
```javascript
const result = await eiip.mergeLayers([
    {
        src: 'background.jpg',
        x: 0,
        y: 0,
        width: 800,
        height: 600,
        zIndex: 1
    },
    {
        src: 'logo.png',
        x: 'center',
        y: 'center',
        scale: 0.3,
        opacity: 0.9,
        rotation: 45,
        zIndex: 2
    }
], {
    format: 'png',
    quality: 0.95
});
```

---

#### `downloadImage(dataUrl, filename)`

Download an image to the user's device.

**Parameters:**
- `dataUrl` - Data URL of the image
- `filename` - Filename for download

**Returns:** `void`

**Example:**
```javascript
const result = await eiip.convertSvgToRaster(svgFile);
eiip.downloadImage(result.dataUrl, 'converted-image.png');
```

---

#### Image Resize Methods (v1.2.0)

##### `resizeImage(input, options)`

Resize image to specific dimensions.

**Example:**
```javascript
const result = await eiip.resizeImage(file, {
    width: 800,
    height: 600,
    fit: 'contain',
    format: 'png',
    quality: 0.92
});
```

##### `scaleImage(input, scaleFactor, options)`

Scale image by percentage.

**Example:**
```javascript
const result = await eiip.scaleImage(file, 0.5); // 50% size
```

##### `createThumbnail(input, options)`

Create thumbnail automatically.

**Example:**
```javascript
const thumb = await eiip.createThumbnail(file, { size: 150 });
```

---

#### Image Rotation Methods (v1.2.0)

##### `rotateImage(input, degrees, options)`

Rotate image by angle.

**Example:**
```javascript
const result = await eiip.rotateImage(file, 90);
```

##### `flipImage(input, direction, options)`

Flip image horizontally or vertically.

**Example:**
```javascript
const result = await eiip.flipImage(file, 'horizontal');
```

---

#### Image Compression Methods (v1.2.0)

##### `compressImage(input, options)`

Compress image to target size.

**Example:**
```javascript
const result = await eiip.compressImage(file, {
    maxSizeKB: 500,
    format: 'webp'
});
```

##### `optimizeImage(input, options)`

Smart optimization for web.

**Example:**
```javascript
const result = await eiip.optimizeImage(file);
```

---

#### Text Overlay Methods (v1.2.0)

##### `addWatermark(input, options)`

Add watermark to image.

**Example:**
```javascript
const result = await eiip.addWatermark(file, {
    text: '© 2024',
    position: 'bottom-right'
});
```

##### `addText(input, options)`

Add custom text overlay.

**Example:**
```javascript
const result = await eiip.addText(file, {
    text: 'Hello World',
    x: 'center',
    y: 'center',
    fontSize: 60
});
```

---

#### Color Adjustment Methods (v1.2.0)

##### `adjustColors(input, options)`

Adjust image colors (HSL).

**Example:**
```javascript
const result = await eiip.adjustColors(file, {
    saturation: 1.3,
    hue: 15,
    lightness: 5,
    vibrance: 1.2
});
```

---

### Static Properties

#### `EIIP.version`
Current library version (string).

#### `EIIP.author`
Library author information (string).

#### `EIIP.SvgToRaster`
Direct access to SvgToRaster class.

#### `EIIP.RasterToSvg`
Direct access to RasterToSvg class.

#### `EIIP.ImageLayerMerger`
Direct access to ImageLayerMerger class.

#### `EIIP.ImageCropper` (v1.1.0)
Direct access to ImageCropper class.

#### `EIIP.ImageEffects` (v1.1.0)
Direct access to ImageEffects class.

#### `EIIP.PDFConverter` (v1.1.0)
Direct access to PDFConverter class.

#### `EIIP.ImageResizer` (v1.2.0)
Direct access to ImageResizer class.

#### `EIIP.ImageRotator` (v1.2.0)
Direct access to ImageRotator class.

#### `EIIP.ImageCompressor` (v1.2.0)
Direct access to ImageCompressor class.

#### `EIIP.TextOverlay` (v1.2.0)
Direct access to TextOverlay class.

#### `EIIP.ColorAdjuster` (v1.2.0)
Direct access to ColorAdjuster class.

**Example:**
```javascript
console.log(EIIP.version); // "1.2.0"

// Use classes directly
const converter = new EIIP.SvgToRaster({ width: 800 });
const result = await converter.convertFromFile(svgFile);

// v1.2.0 classes
const resizer = new EIIP.ImageResizer();
const resized = await resizer.resize(file, { width: 800, height: 600 });
```

---

## SVG to Raster Conversion

### SvgToRaster Class

Direct access to SVG conversion functionality.

```javascript
const converter = new EIIP.SvgToRaster(options);
```

#### Methods

- `convertFromFile(file)` - Convert from File object
- `convertFromUrl(url)` - Convert from URL
- `convertFromSvgString(svgString)` - Convert from SVG string
- `convertFromSvgElement(svgElement)` - Convert from SVG DOM element
- `convertToKnittingDesign(svgString, conversionOptions, knittingOptions)` - Convert with knitting effects

---

### SVG Conversion Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `width` | `number` | `null` | Output width in pixels |
| `height` | `number` | `null` | Output height in pixels |
| `format` | `string` | `'png'` | Output format: `'png'`, `'jpg'`, `'jpeg'`, `'webp'` |
| `quality` | `number` | `0.92` | Output quality (0.0 - 1.0) |
| `backgroundColor` | `string` | `'transparent'` | Background color (CSS color or 'transparent') |
| `scaleFactor` | `number` | `1.0` | Scale multiplier (e.g., 2.0 for 2x size) |
| `preserveAspectRatio` | `boolean` | `true` | Maintain aspect ratio when resizing |
| `debug` | `boolean` | `false` | Enable debug logging |

#### Quality Presets

```javascript
EIIP.SvgToRaster.QUALITY_PRESETS = {
    LOW: 0.6,
    MEDIUM: 0.75,
    HIGH: 0.92,
    MAXIMUM: 1.0
};
```

#### Format Constants

```javascript
EIIP.SvgToRaster.OUTPUT_FORMATS = {
    PNG: 'png',
    JPG: 'jpg',
    JPEG: 'jpeg',
    WEBP: 'webp'
};
```

**Example:**
```javascript
const result = await eiip.convertSvgToRaster(svgFile, {
    width: 1920,
    height: 1080,
    format: 'webp',
    quality: EIIP.SvgToRaster.QUALITY_PRESETS.HIGH,
    backgroundColor: '#ffffff',
    scaleFactor: 1.5,
    preserveAspectRatio: true
});
```

---

### Knitting Effects

Add realistic knitting textures to SVG conversions.

#### Knitting Patterns

```javascript
EIIP.SvgToRaster.KNITTING_PATTERNS = {
    BASIC: 'basic',           // Standard stockinette stitch
    CABLE: 'cable',           // Cable knit pattern
    RIBBED: 'ribbed',         // Ribbed knit pattern
    SEED: 'seed',             // Seed stitch pattern
    HERRINGBONE: 'herringbone', // Herringbone pattern
    LACE: 'lace',             // Delicate lace pattern
    FAIR_ISLE: 'fairisle',    // Traditional Fair Isle
    BASKETWEAVE: 'basketweave', // Woven basket texture
    HONEYCOMB: 'honeycomb',   // Honeycomb texture
    DIAGONAL: 'diagonal'      // Diagonal rib pattern
};
```

#### Yarn Types

```javascript
EIIP.SvgToRaster.YARN_TYPES = {
    WOOL: 'wool',
    COTTON: 'cotton',
    ACRYLIC: 'acrylic',
    SILK: 'silk',
    CASHMERE: 'cashmere',
    ALPACA: 'alpaca'
};
```

#### Convert with Knitting Effect

```javascript
const converter = new EIIP.SvgToRaster();
const result = await converter.convertToKnittingDesign(
    svgString,
    {
        width: 800,
        height: 600,
        format: 'png'
    },
    {
        pattern: EIIP.SvgToRaster.KNITTING_PATTERNS.CABLE,
        yarnType: EIIP.SvgToRaster.YARN_TYPES.WOOL,
        needleSize: 4,
        gauge: 'standard',
        colorCount: 4
    }
);
```

---

## Raster to SVG Conversion

### RasterToSvg Class

Direct access to raster-to-SVG conversion functionality.

```javascript
const converter = new EIIP.RasterToSvg(options);
```

#### Methods

- `convertFromFile(file)` - Convert from File object
- `convertFromUrl(url)` - Convert from URL
- `convertFromDataUrl(dataUrl)` - Convert from data URL
- `convertFromImageElement(imgElement)` - Convert from image element

---

### Raster Conversion Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `scaleFactor` | `number` | `1.0` | Scale output SVG (0.1 - 10.0) |
| `colorReductionLevel` | `number` | `0` | Color reduction level (0-24, see [Color Reduction](#color-reduction)) |
| `exactColorCount` | `number` | `null` | Exact number of colors to use |
| `debug` | `boolean` | `false` | Enable debug logging |

**Example:**
```javascript
const result = await eiip.convertRasterToSvg(imageFile, {
    scaleFactor: 0.5,        // 50% size
    colorReductionLevel: 16, // ~4,000 colors
    debug: true
});
```

---

### Color Reduction

Control the number of colors in the output SVG.

#### Reduction Levels

| Level | Approximate Colors | Use Case |
|-------|-------------------|----------|
| 0 | 16,777,216 (24-bit) | Full color, no reduction |
| 1 | 32,768 | High color fidelity |
| 2 | 4,096 | Good balance |
| 4 | 512 | Medium reduction |
| 8 | 64 | Noticeable reduction |
| 16 | 8 | Heavy reduction |
| 24 | 2 | Minimal colors |

#### Color Presets

```javascript
EIIP.RasterToSvg.COLOR_PRESETS = {
    FULL_COLOR: 0,           // 16M+ colors
    HIGH_QUALITY: 1,         // 32K colors
    MEDIUM_QUALITY: 4,       // 512 colors
    LOW_QUALITY: 8,          // 64 colors
    MINIMAL: 16,             // 8 colors
    BINARY: 24               // 2 colors
};
```

**Example:**
```javascript
// Using preset
const result1 = await eiip.convertRasterToSvg(imageFile, {
    colorReductionLevel: EIIP.RasterToSvg.COLOR_PRESETS.MEDIUM_QUALITY
});

// Using exact color count
const result2 = await eiip.convertRasterToSvg(imageFile, {
    exactColorCount: 256  // Exactly 256 colors
});

// Get available color counts
const colorCounts = EIIP.RasterToSvg.getAvailableColorCounts();
console.log(colorCounts); // [16777216, 32768, 4096, 512, ...]
```

---

## Image Layer Merging

### ImageLayerMerger Class

Direct access to layer merging functionality.

```javascript
const merger = new EIIP.ImageLayerMerger(options);
```

#### Constructor Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `width` | `number` | `800` | Output canvas width |
| `height` | `number` | `600` | Output canvas height |
| `backgroundColor` | `string` | `'transparent'` | Background color |
| `debug` | `boolean` | `false` | Enable debug logging |

---

### Layer Options

Each layer object can have the following properties:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `src` | `string\|File\|Blob` | **required** | Image source (URL, data URL, File, or Blob) |
| `x` | `number\|string` | `0` | X position (pixels or 'center') |
| `y` | `number\|string` | `0` | Y position (pixels or 'center') |
| `width` | `number` | auto | Layer width (pixels) |
| `height` | `number` | auto | Layer height (pixels) |
| `scale` | `number` | `1.0` | Scale factor (0.1 - 10.0) |
| `opacity` | `number` | `1.0` | Opacity (0.0 - 1.0) |
| `rotation` | `number` | `0` | Rotation in degrees |
| `zIndex` | `number` | `0` | Layer stacking order (higher = front) |
| `blendMode` | `string` | `'source-over'` | Canvas blend mode |

#### Position Options

- **Numeric:** Absolute pixel position (e.g., `x: 100`)
- **'center':** Center the layer on that axis
- **Relative:** Not yet implemented

**Example:**
```javascript
const layers = [
    // Background layer
    {
        src: '/images/background.jpg',
        x: 0,
        y: 0,
        width: 1920,
        height: 1080,
        zIndex: 1
    },
    // Centered logo
    {
        src: '/images/logo.png',
        x: 'center',
        y: 'center',
        scale: 0.5,
        opacity: 0.9,
        zIndex: 2
    },
    // Watermark
    {
        src: '/images/watermark.png',
        x: 1700,
        y: 950,
        scale: 0.3,
        opacity: 0.5,
        rotation: -15,
        zIndex: 3
    }
];

const result = await eiip.mergeLayers(layers);
```

---

### Merge Options

Options passed to the `mergeLayers()` method:

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `format` | `string` | `'png'` | Output format: `'png'`, `'jpg'`, `'jpeg'`, `'webp'` |
| `quality` | `number` | `0.9` | Output quality (0.0 - 1.0) |
| `dpi` | `number` | `72` | DPI for print quality |

**Example:**
```javascript
const result = await eiip.mergeLayers(layers, {
    format: 'webp',
    quality: 0.95,
    dpi: 300  // Print quality
});
```

---

## Utility Methods

### Download Helper

```javascript
eiip.downloadImage(dataUrl, filename);
```

Downloads the image to the user's device.

### Canvas to Blob

Available on converter instances:

```javascript
const converter = new EIIP.SvgToRaster();
const blob = await converter.convertToBlob(dataUrl);
```

---

## Error Handling

All async methods return Promises and should be wrapped in try-catch blocks.

```javascript
try {
    const result = await eiip.convertSvgToRaster(svgFile, {
        width: 800,
        height: 600
    });
    
    console.log('Conversion successful:', result);
    
} catch (error) {
    console.error('Conversion failed:', error.message);
    
    // Handle specific errors
    if (error.message.includes('Invalid file type')) {
        alert('Please provide a valid SVG file');
    } else if (error.message.includes('Failed to load')) {
        alert('Could not load the image');
    } else {
        alert('An error occurred during conversion');
    }
}
```

### Common Error Messages

- `"Invalid file type"` - Wrong file format provided
- `"Failed to load image"` - Image could not be loaded
- `"Invalid SVG format"` - SVG parsing error
- `"Conversion failed"` - General conversion error
- `"Failed to read file"` - File reading error

---

## Browser Compatibility

EIIP works in all modern browsers that support:
- Canvas API
- File API
- Fetch API
- Promises/async-await
- ES6 features

### Minimum Browser Versions

- Chrome 60+
- Firefox 55+
- Safari 11+
- Edge 79+
- Opera 47+

### Node.js Compatibility

EIIP can be used in Node.js environments with a DOM implementation like `jsdom` or `canvas`.

```javascript
// Node.js example with canvas
const { createCanvas, loadImage } = require('canvas');
const EIIP = require('eiip');

// Use with node-canvas polyfills
```

---

## Framework Integration Examples

### React

```jsx
import React, { useState } from 'react';
import EIIP from 'eiip';

function ImageConverter() {
    const [result, setResult] = useState(null);
    const eiip = new EIIP();

    const handleConvert = async (file) => {
        try {
            const converted = await eiip.convertSvgToRaster(file, {
                width: 800,
                format: 'png'
            });
            setResult(converted.dataUrl);
        } catch (error) {
            console.error('Conversion failed:', error);
        }
    };

    return (
        <div>
            <input 
                type="file" 
                accept=".svg"
                onChange={(e) => handleConvert(e.target.files[0])} 
            />
            {result && <img src={result} alt="Converted" />}
        </div>
    );
}
```

### Vue.js

```vue
<template>
    <div>
        <input type="file" @change="handleConvert" accept=".svg">
        <img v-if="result" :src="result" alt="Converted">
    </div>
</template>

<script>
import EIIP from 'eiip';

export default {
    data() {
        return {
            result: null,
            eiip: new EIIP()
        };
    },
    methods: {
        async handleConvert(event) {
            try {
                const file = event.target.files[0];
                const converted = await this.eiip.convertSvgToRaster(file, {
                    width: 800,
                    format: 'png'
                });
                this.result = converted.dataUrl;
            } catch (error) {
                console.error('Conversion failed:', error);
            }
        }
    }
};
</script>
```

### Angular

```typescript
import { Component } from '@angular/core';
import EIIP from 'eiip';

@Component({
    selector: 'app-converter',
    template: `
        <input type="file" (change)="handleConvert($event)" accept=".svg">
        <img *ngIf="result" [src]="result" alt="Converted">
    `
})
export class ConverterComponent {
    result: string | null = null;
    eiip = new EIIP();

    async handleConvert(event: Event) {
        try {
            const file = (event.target as HTMLInputElement).files?.[0];
            if (file) {
                const converted = await this.eiip.convertSvgToRaster(file, {
                    width: 800,
                    format: 'png'
                });
                this.result = converted.dataUrl;
            }
        } catch (error) {
            console.error('Conversion failed:', error);
        }
    }
}
```

---

## Advanced Usage

### Batch Processing

```javascript
const eiip = new EIIP();

async function batchConvert(files) {
    const results = await Promise.all(
        files.map(file => eiip.convertSvgToRaster(file, {
            width: 800,
            format: 'png'
        }))
    );
    return results;
}
```

### Progressive Enhancement

```javascript
const eiip = new EIIP({ debug: true });

// Low quality preview
const preview = await eiip.convertSvgToRaster(svgFile, {
    width: 400,
    quality: 0.6
});
showPreview(preview.dataUrl);

// High quality final
const final = await eiip.convertSvgToRaster(svgFile, {
    width: 2000,
    quality: 0.95
});
showFinal(final.dataUrl);
```

### Custom Pipeline

```javascript
// Convert, merge, and download
async function processAndDownload(svgFile, logoFile) {
    const eiip = new EIIP();
    
    // Convert SVG to raster
    const converted = await eiip.convertSvgToRaster(svgFile, {
        width: 1920,
        height: 1080,
        format: 'png'
    });
    
    // Merge with logo
    const merged = await eiip.mergeLayers([
        { src: converted.dataUrl, x: 0, y: 0, zIndex: 1 },
        { src: logoFile, x: 'center', y: 50, scale: 0.3, zIndex: 2 }
    ], {
        format: 'png',
        quality: 0.95
    });
    
    // Download result
    eiip.downloadImage(merged.dataUrl, 'final-image.png');
}
```

---

## Image Resize & Scale (v1.2.0)

### ImageResizer Class

Resize and scale images with quality preservation.

#### Methods

##### `resize(input, options)`

Resize image to specific dimensions with fit modes.

**Parameters:**
- `input` - Image source (File, Blob, data URL, canvas, or image element)
- `options` - Configuration object:
  - `width` (number) - Target width in pixels
  - `height` (number) - Target height in pixels
  - `fit` (string) - Fit mode: 'contain', 'cover', or 'fill' (default: 'contain')
  - `format` (string) - Output format: 'png', 'jpeg', 'webp' (default: 'png')
  - `quality` (number) - Quality 0-1 (default: 0.92)

**Returns:** Promise resolving to result object with canvas, dataUrl, blob, width, height

**Example:**
```javascript
const resizer = new EIIP.ImageResizer();

// Contain - fit inside dimensions
const result = await resizer.resize(file, {
    width: 800,
    height: 600,
    fit: 'contain'
});

// Cover - fill area, crop if needed
const result2 = await resizer.resize(file, {
    width: 800,
    height: 600,
    fit: 'cover'
});

// Fill - stretch to fill
const result3 = await resizer.resize(file, {
    width: 800,
    height: 600,
    fit: 'fill'
});
```

##### `scale(input, scaleFactor, options)`

Scale image by percentage.

**Parameters:**
- `input` - Image source
- `scaleFactor` (number) - Scale factor (0.5 = 50%, 2.0 = 200%)
- `options` - Configuration object:
  - `format` (string) - Output format (default: 'png')
  - `quality` (number) - Quality 0-1 (default: 0.92)

**Example:**
```javascript
// Scale to 50%
const result = await resizer.scale(file, 0.5);

// Scale to 200%
const result2 = await resizer.scale(file, 2.0);
```

##### `createThumbnail(input, options)`

Create thumbnail automatically.

**Parameters:**
- `input` - Image source
- `options` - Configuration object:
  - `size` (number) - Thumbnail size (width & height) in pixels (default: 150)
  - `format` (string) - Output format (default: 'webp')
  - `quality` (number) - Quality 0-1 (default: 0.85)

**Example:**
```javascript
const thumb = await resizer.createThumbnail(file, {
    size: 200,
    format: 'webp',
    quality: 0.85
});
```

#### Constants

```javascript
EIIP.ImageResizer.FIT_MODES = {
    CONTAIN: 'contain',
    COVER: 'cover',
    FILL: 'fill'
}
```

---

## Image Rotation & Flip (v1.2.0)

### ImageRotator Class

Rotate and flip images.

#### Methods

##### `rotate(input, degrees, options)`

Rotate image by specified angle.

**Parameters:**
- `input` - Image source
- `degrees` (number) - Rotation angle (90, 180, 270, or custom)
- `options` - Configuration object:
  - `format` (string) - Output format (default: 'png')
  - `quality` (number) - Quality 0-1 (default: 0.92)

**Example:**
```javascript
const rotator = new EIIP.ImageRotator();

// Rotate 90 degrees
const result = await rotator.rotate(file, 90);

// Rotate 180 degrees
const result2 = await rotator.rotate(file, 180);

// Rotate 270 degrees
const result3 = await rotator.rotate(file, 270);
```

##### `flip(input, direction, options)`

Flip image horizontally or vertically.

**Parameters:**
- `input` - Image source
- `direction` (string) - 'horizontal' or 'vertical'
- `options` - Configuration object:
  - `format` (string) - Output format (default: 'png')
  - `quality` (number) - Quality 0-1 (default: 0.92)

**Example:**
```javascript
// Flip horizontal
const result = await rotator.flip(file, 'horizontal');

// Flip vertical
const result2 = await rotator.flip(file, 'vertical');
```

#### Constants

```javascript
EIIP.ImageRotator.DIRECTIONS = {
    HORIZONTAL: 'horizontal',
    VERTICAL: 'vertical'
}
```

---

## Image Compression (v1.2.0)

### ImageCompressor Class

Smart image compression and optimization.

#### Methods

##### `compress(input, options)`

Compress image to target file size.

**Parameters:**
- `input` - Image source
- `options` - Configuration object:
  - `maxSizeKB` (number) - Target maximum size in KB (default: 500)
  - `format` (string) - Output format: 'webp', 'jpeg', 'png' (default: 'jpeg')
  - `quality` (number) - Initial quality 0-1 (default: 0.9)
  - `maxWidth` (number) - Optional max width
  - `maxHeight` (number) - Optional max height

**Returns:** Promise with sizeKB, quality, compressionRatio, and standard outputs

**Example:**
```javascript
const compressor = new EIIP.ImageCompressor();

// Compress to under 500KB
const result = await compressor.compress(file, {
    maxSizeKB: 500,
    format: 'webp'
});

console.log(`Compressed to ${result.sizeKB}KB`);
console.log(`Quality: ${result.quality}`);
console.log(`Savings: ${result.compressionRatio}%`);
```

##### `optimize(input, options)`

Smart optimization with defaults for web delivery.

**Parameters:**
- `input` - Image source
- `options` - Configuration object:
  - `maxSizeKB` (number) - Target size (default: 800)
  - `maxWidth` (number) - Max width (default: 1920)
  - `maxHeight` (number) - Max height (default: 1080)
  - `format` (string) - Format (default: 'webp')
  - `quality` (number) - Quality (default: 0.85)

**Example:**
```javascript
// Smart optimize for web
const result = await compressor.optimize(file);
```

---

## Text Overlay & Watermark (v1.2.0)

### TextOverlay Class

Add watermarks and text to images.

#### Methods

##### `addWatermark(input, options)`

Add watermark text to image.

**Parameters:**
- `input` - Image source
- `options` - Configuration object:
  - `text` (string) - Watermark text (default: '© Watermark')
  - `position` (string) - Position preset (default: 'bottom-right')
  - `fontSize` (number) - Font size (auto-calculated by default)
  - `fontFamily` (string) - Font family (default: 'Arial')
  - `color` (string) - Text color (default: '#ffffff')
  - `opacity` (number) - Text opacity 0-1 (default: 0.5)
  - `shadow` (boolean) - Add shadow (default: false)
  - `padding` (number) - Padding from edge (default: 20)
  - `format` (string) - Output format (default: 'png')
  - `quality` (number) - Quality 0-1 (default: 0.92)

**Example:**
```javascript
const overlay = new EIIP.TextOverlay();

// Add watermark
const result = await overlay.addWatermark(file, {
    text: '© 2024 Your Company',
    position: 'bottom-right',
    opacity: 0.7,
    color: '#ffffff',
    shadow: true
});
```

##### `addText(input, options)`

Add custom text overlay.

**Parameters:**
- `input` - Image source
- `options` - Configuration object:
  - `text` (string) - Text to add (default: 'Text')
  - `fontSize` (number) - Font size (default: 48)
  - `fontFamily` (string) - Font family (default: 'Arial')
  - `color` (string) - Text color (default: '#000000')
  - `backgroundColor` (string) - Optional background color
  - `x` (number|'center') - X position (default: 0)
  - `y` (number|'center') - Y position (default: 50)
  - `align` (string) - Text alignment: 'left', 'center', 'right' (default: 'left')
  - `bold` (boolean) - Bold text (default: false)
  - `italic` (boolean) - Italic text (default: false)
  - `shadow` (boolean) - Add shadow (default: false)
  - `shadowColor` (string) - Shadow color (default: 'rgba(0,0,0,0.5)')
  - `shadowBlur` (number) - Shadow blur (default: 4)
  - `shadowOffsetX` (number) - Shadow X offset (default: 2)
  - `shadowOffsetY` (number) - Shadow Y offset (default: 2)
  - `stroke` (boolean) - Add text stroke (default: false)
  - `strokeColor` (string) - Stroke color (default: '#ffffff')
  - `strokeWidth` (number) - Stroke width (default: 2)
  - `format` (string) - Output format (default: 'png')
  - `quality` (number) - Quality 0-1 (default: 0.92)

**Example:**
```javascript
// Add centered text
const result = await overlay.addText(file, {
    text: 'Hello World',
    fontSize: 60,
    color: '#000000',
    x: 'center',
    y: 'center',
    bold: true,
    shadow: true,
    stroke: true,
    strokeColor: '#ffffff'
});
```

#### Constants

```javascript
EIIP.TextOverlay.POSITIONS = {
    TOP_LEFT: 'top-left',
    TOP_RIGHT: 'top-right',
    BOTTOM_LEFT: 'bottom-left',
    BOTTOM_RIGHT: 'bottom-right',
    CENTER: 'center'
}
```

---

## Color Adjustment (v1.2.0)

### ColorAdjuster Class

Advanced color manipulation using HSL color space.

#### Methods

##### `adjustColors(input, options)`

Adjust multiple color properties.

**Parameters:**
- `input` - Image source
- `options` - Configuration object:
  - `saturation` (number) - Saturation multiplier 0-2 (1 = no change, default: 1.0)
  - `hue` (number) - Hue rotation in degrees -180 to 180 (default: 0)
  - `lightness` (number) - Lightness adjustment -50 to 50 (default: 0)
  - `vibrance` (number) - Vibrance multiplier 0-2 (1 = no change, default: 1.0)
  - `format` (string) - Output format (default: 'png')
  - `quality` (number) - Quality 0-1 (default: 0.92)

**Returns:** Promise with adjustments object containing applied values

**Example:**
```javascript
const adjuster = new EIIP.ColorAdjuster();

// Enhance colors
const result = await adjuster.adjustColors(file, {
    saturation: 1.3,    // Increase saturation by 30%
    hue: 15,            // Warm color shift
    lightness: 5,       // Slightly brighter
    vibrance: 1.2       // Enhance vibrance
});

// Desaturate (black & white)
const bw = await adjuster.adjustColors(file, {
    saturation: 0
});

// Cool tone
const cool = await adjuster.adjustColors(file, {
    hue: -30,           // Blue shift
    saturation: 0.9,
    lightness: 5
});
```

**Color Parameters Explained:**
- **Saturation**: Controls color intensity
  - 0 = Grayscale (no color)
  - 1 = Original colors
  - 2 = Maximum saturation
  
- **Hue**: Rotates colors around color wheel
  - -180 to -90 = Cool tones (blue/cyan)
  - 0 = Original colors
  - +90 to +180 = Warm tones (red/orange)
  
- **Lightness**: Adjusts brightness
  - -50 = Much darker
  - 0 = Original brightness
  - +50 = Much lighter
  
- **Vibrance**: Selective saturation (boosts muted colors more)
  - 0 = Desaturated
  - 1 = Original
  - 2 = Highly vibrant

---

## Performance Tips

1. **Use appropriate dimensions** - Don't render larger than needed
2. **Choose the right format** - WebP for smaller file sizes, PNG for quality
3. **Optimize quality settings** - 0.85-0.92 is usually sufficient
4. **Scale efficiently** - Use `scaleFactor` instead of resizing after
5. **Batch process** - Use `Promise.all()` for multiple conversions
6. **Enable debug mode** - Only during development
7. **Cleanup** - Results contain canvas elements that can be garbage collected

---

## Support

- **GitHub Issues:** [EIIP-Frontend Issues](https://github.com/SaleemLww/EIIP-Frontend/issues)
- **NPM Package:** [npmjs.com/package/eiip](https://www.npmjs.com/package/eiip)
- **Repository:** [github.com/SaleemLww/EIIP-Frontend](https://github.com/SaleemLww/EIIP-Frontend)

---

## License

MIT License - Copyright (c) 2025 Saleem Ahmad (Elite India)

---

**EIIP** - Elite India Image Processing Library  
Making client-side image processing simple and powerful! 🎨✨
