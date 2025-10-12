# EIIP API Reference

Complete API documentation for EIIP (Elite India Image Processing) library.

**Version:** 1.0.1  
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

**Example:**
```javascript
console.log(EIIP.version); // "1.0.1"

// Use classes directly
const converter = new EIIP.SvgToRaster({ width: 800 });
const result = await converter.convertFromFile(svgFile);
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
