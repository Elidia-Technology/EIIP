# EIIP New Features Examples

Examples demonstrating the new image cropping, effects, and PDF conversion features in EIIP v1.1.0.

---

## Image Cropping

### Basic Rectangle Crop

```javascript
const eiip = new EIIP({ debug: true });

// Crop a specific area
const result = await eiip.cropImage(imageFile, {
    x: 100,
    y: 100,
    width: 400,
    height: 300
}, {
    format: 'png',
    quality: 0.9
});

document.getElementById('output').src = result.dataUrl;
```

### Circle Crop (Profile Pictures)

```javascript
// Create circular crop
const result = await eiip.cropToCircle(imageFile, {
    size: 500,      // Circle diameter
    x: 200,         // Center X in original image
    y: 200,         // Center Y in original image
    format: 'png'
});

document.getElementById('avatar').src = result.dataUrl;
```

### Aspect Ratio Crop

```javascript
// Crop to 16:9 aspect ratio
const result = await eiip.cropToAspectRatio(imageFile, 
    EIIP.ImageCropper.ASPECT_RATIOS.LANDSCAPE,
    { format: 'jpg', quality: 0.9 }
);

// Available aspect ratios:
// SQUARE: 1:1
// LANDSCAPE: 16:9
// PORTRAIT: 9:16
// CLASSIC: 4:3
// WIDE: 21:9
// INSTAGRAM: 4:5
// FACEBOOK_COVER: 820:312

// Custom aspect ratio
const customResult = await eiip.cropToAspectRatio(imageFile, 
    { width: 21, height: 9 },
    { format: 'webp' }
);
```

---

## Image Effects & Filters

### Apply Single Effect

```javascript
const eiip = new EIIP({ debug: true });

// Grayscale effect
const grayscale = await eiip.applyEffect(imageFile, 'grayscale', 1.0, {
    format: 'jpg',
    quality: 0.9
});

// Sepia effect
const sepia = await eiip.applyEffect(imageFile, 'sepia', 0.8);

// Blur effect
const blur = await eiip.applyEffect(imageFile, 'blur', 0.5);

// Available effects:
// - GRAYSCALE
// - SEPIA
// - BRIGHTNESS (intensity: 0.0-1.0, 0.5 = normal)
// - CONTRAST (intensity: 0.0-2.0, 1.0 = normal)
// - INVERT
// - BLUR
// - SHARPEN
// - VINTAGE
// - WARM
// - COOL
```

### Effect Intensity Control

```javascript
// Full effect (intensity = 1.0)
const fullEffect = await eiip.applyEffect(image, 'sepia', 1.0);

// Subtle effect (intensity = 0.3)
const subtleEffect = await eiip.applyEffect(image, 'sepia', 0.3);

// Very subtle (intensity = 0.1)
const verySubtle = await eiip.applyEffect(image, 'warm', 0.1);
```

### Apply Multiple Effects

```javascript
// Chain multiple effects
const result = await eiip.applyEffects(imageFile, [
    { name: 'brightness', intensity: 1.2 },
    { name: 'contrast', intensity: 1.1 },
    { name: 'warm', intensity: 0.3 }
], {
    format: 'jpg',
    quality: 0.95
});

// Create vintage look
const vintage = await eiip.applyEffects(imageFile, [
    { name: 'sepia', intensity: 0.6 },
    { name: 'contrast', intensity: 0.9 },
    { name: 'vintage', intensity: 0.8 }
]);
```

### All Available Effects

```javascript
// Get list of all effects
const effects = EIIP.ImageEffects.EFFECT_NAMES;
console.log(effects);
// ['GRAYSCALE', 'SEPIA', 'BRIGHTNESS', 'CONTRAST', 'INVERT', 
//  'BLUR', 'SHARPEN', 'VINTAGE', 'WARM', 'COOL']

// Apply each effect
for (const effect of effects) {
    const result = await eiip.applyEffect(imageFile, effect, 1.0);
    // Display result...
}
```

---

## PDF Conversion

### Convert Single Image to PDF

```javascript
const eiip = new EIIP({ debug: true });

// Convert image to PDF
const pdf = await eiip.convertImageToPDF(imageFile, {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.A4,
    orientation: 'portrait',
    margin: 20,
    imageOptions: {
        fit: 'contain'  // 'contain', 'cover', or 'fill'
    }
});

// Download PDF
const link = document.createElement('a');
link.href = pdf.downloadUrl;
link.download = 'document.pdf';
link.click();

console.log(`PDF created with ${pdf.pageCount} page(s)`);
```

### Convert Multiple Images to PDF

```javascript
// Multiple images, one per page
const images = [image1, image2, image3];

const pdf = await eiip.convertImagesToPDF(images, {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.LETTER,
    orientation: 'portrait',
    margin: 30
});

// Download multi-page PDF
eiip.downloadImage(pdf.downloadUrl, 'multi-page.pdf');
```

### Advanced PDF Options

```javascript
// With custom options per image
const inputs = [
    {
        src: image1,
        options: { fit: 'contain' }
    },
    {
        src: image2,
        options: { fit: 'cover' }
    },
    {
        src: image3,
        options: { fit: 'fill' }
    }
];

const pdf = await eiip.convertImagesToPDF(inputs, {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.A3,
    orientation: 'landscape',
    margin: 40
});
```

### Available Page Sizes

```javascript
// Standard page sizes
EIIP.PDFConverter.PAGE_SIZES.A4        // 595 x 842 points
EIIP.PDFConverter.PAGE_SIZES.LETTER    // 612 x 792 points
EIIP.PDFConverter.PAGE_SIZES.LEGAL     // 612 x 1008 points
EIIP.PDFConverter.PAGE_SIZES.A3        // 842 x 1191 points
EIIP.PDFConverter.PAGE_SIZES.A5        // 420 x 595 points
EIIP.PDFConverter.PAGE_SIZES.TABLOID   // 792 x 1224 points

// Orientations
EIIP.PDFConverter.ORIENTATIONS.PORTRAIT
EIIP.PDFConverter.ORIENTATIONS.LANDSCAPE
```

---

## Combined Workflows

### Crop, Apply Effect, Then Convert to PDF

```javascript
const eiip = new EIIP({ debug: true });

// 1. Crop image
const cropped = await eiip.cropToAspectRatio(
    originalImage,
    EIIP.ImageCropper.ASPECT_RATIOS.LANDSCAPE
);

// 2. Apply vintage effect
const withEffect = await eiip.applyEffects(cropped.dataUrl, [
    { name: 'sepia', intensity: 0.7 },
    { name: 'vintage', intensity: 0.6 }
]);

// 3. Convert to PDF
const pdf = await eiip.convertImageToPDF(withEffect.dataUrl, {
    pageSize: EIIP.PDFConverter.PAGE_SIZES.A4,
    orientation: 'landscape'
});

// 4. Download
eiip.downloadImage(pdf.downloadUrl, 'final-document.pdf');
```

### Create Social Media Images

```javascript
// Instagram post (1:1)
const instagramPost = await eiip.cropToAspectRatio(
    image,
    EIIP.ImageCropper.ASPECT_RATIOS.SQUARE,
    { format: 'jpg', quality: 0.9 }
);

// Instagram story (9:16)
const instagramStory = await eiip.cropToAspectRatio(
    image,
    EIIP.ImageCropper.ASPECT_RATIOS.PORTRAIT,
    { format: 'jpg', quality: 0.9 }
);

// Facebook cover
const facebookCover = await eiip.cropToAspectRatio(
    image,
    EIIP.ImageCropper.ASPECT_RATIOS.FACEBOOK_COVER,
    { format: 'jpg', quality: 0.85 }
);
```

### Batch Process with Effects

```javascript
const images = [image1, image2, image3, image4];
const effects = ['grayscale', 'sepia', 'vintage', 'warm'];

const results = await Promise.all(
    images.map(async (image, index) => {
        return await eiip.applyEffect(
            image,
            effects[index],
            0.8,
            { format: 'jpg' }
        );
    })
);

// Display all results
results.forEach((result, index) => {
    document.getElementById(`output-${index}`).src = result.dataUrl;
});
```

### Profile Picture Pipeline

```javascript
// Complete profile picture processing
async function createProfilePicture(imageFile) {
    const eiip = new EIIP({ debug: true });
    
    // 1. Crop to square
    const squared = await eiip.cropToAspectRatio(
        imageFile,
        EIIP.ImageCropper.ASPECT_RATIOS.SQUARE,
        { format: 'png' }
    );
    
    // 2. Crop to circle
    const circular = await eiip.cropToCircle(squared.canvas, {
        size: 500,
        format: 'png'
    });
    
    // 3. Apply subtle warm effect
    const enhanced = await eiip.applyEffect(
        circular.dataUrl,
        'warm',
        0.2,
        { format: 'png', quality: 0.95 }
    );
    
    return enhanced;
}

// Use it
const profilePic = await createProfilePicture(uploadedImage);
document.getElementById('profile-avatar').src = profilePic.dataUrl;
```

---

## React Integration

### Image Cropper Component

```jsx
import React, { useState } from 'react';
import EIIP from 'eiip';

function ImageCropper() {
    const [original, setOriginal] = useState(null);
    const [cropped, setCropped] = useState(null);
    const eiip = new EIIP();

    const handleCrop = async () => {
        if (!original) return;
        
        const result = await eiip.cropToCircle(original, {
            size: 400,
            format: 'png'
        });
        
        setCropped(result.dataUrl);
    };

    return (
        <div>
            <input 
                type="file" 
                accept="image/*"
                onChange={(e) => setOriginal(e.target.files[0])} 
            />
            <button onClick={handleCrop}>Crop to Circle</button>
            {cropped && <img src={cropped} alt="Cropped" />}
        </div>
    );
}
```

### Effects Filter Component

```jsx
import React, { useState, useEffect } from 'react';
import EIIP from 'eiip';

function ImageEffects() {
    const [image, setImage] = useState(null);
    const [effect, setEffect] = useState('grayscale');
    const [result, setResult] = useState(null);
    const eiip = new EIIP();

    useEffect(() => {
        if (image && effect) {
            applyEffect();
        }
    }, [image, effect]);

    const applyEffect = async () => {
        const processed = await eiip.applyEffect(image, effect, 1.0);
        setResult(processed.dataUrl);
    };

    const effects = EIIP.ImageEffects.EFFECT_NAMES;

    return (
        <div>
            <input 
                type="file" 
                accept="image/*"
                onChange={(e) => setImage(e.target.files[0])} 
            />
            
            <select value={effect} onChange={(e) => setEffect(e.target.value)}>
                {effects.map(eff => (
                    <option key={eff} value={eff.toLowerCase()}>
                        {eff}
                    </option>
                ))}
            </select>
            
            {result && <img src={result} alt="With Effect" />}
        </div>
    );
}
```

### PDF Converter Component

```jsx
import React, { useState } from 'react';
import EIIP from 'eiip';

function PDFCreator() {
    const [images, setImages] = useState([]);
    const [creating, setCreating] = useState(false);
    const eiip = new EIIP();

    const handleCreatePDF = async () => {
        setCreating(true);
        try {
            const pdf = await eiip.convertImagesToPDF(images, {
                pageSize: EIIP.PDFConverter.PAGE_SIZES.A4,
                orientation: 'portrait'
            });
            
            eiip.downloadImage(pdf.downloadUrl, 'document.pdf');
        } catch (error) {
            console.error('PDF creation failed:', error);
        } finally {
            setCreating(false);
        }
    };

    return (
        <div>
            <input 
                type="file" 
                accept="image/*"
                multiple
                onChange={(e) => setImages(Array.from(e.target.files))} 
            />
            
            <button onClick={handleCreatePDF} disabled={creating || images.length === 0}>
                {creating ? 'Creating PDF...' : `Create PDF (${images.length} images)`}
            </button>
        </div>
    );
}
```

---

## Direct Class Usage

### Using ImageCropper Directly

```javascript
const cropper = new EIIP.ImageCropper({ debug: true });

// Rectangle crop
const result1 = await cropper.crop(image, {
    x: 50, y: 50, width: 300, height: 200
});

// Circle crop
const result2 = await cropper.cropToCircle(image, { size: 400 });

// Aspect ratio crop
const result3 = await cropper.cropToAspectRatio(
    image,
    EIIP.ImageCropper.ASPECT_RATIOS.LANDSCAPE
);
```

### Using ImageEffects Directly

```javascript
const effects = new EIIP.ImageEffects({ debug: true });

// Single effect
const result1 = await effects.applyEffect(image, 'vintage', 0.8);

// Multiple effects
const result2 = await effects.applyEffects(image, [
    { name: 'brightness', intensity: 1.2 },
    { name: 'contrast', intensity: 1.1 }
]);
```

### Using PDFConverter Directly

```javascript
const converter = new EIIP.PDFConverter({
    debug: true,
    pageSize: EIIP.PDFConverter.PAGE_SIZES.LETTER,
    orientation: 'portrait',
    margin: 30
});

// Single image
const pdf1 = await converter.convertImageToPDF(image);

// Multiple images
const pdf2 = await converter.convertImagesToPDF([img1, img2, img3]);
```

---

## Performance Tips

1. **Process in sequence for large batches**
   ```javascript
   // Instead of Promise.all for 100+ images
   for (const image of images) {
       const result = await eiip.applyEffect(image, 'grayscale');
       // Process result...
   }
   ```

2. **Use appropriate quality settings**
   ```javascript
   // For web display
   { format: 'jpg', quality: 0.85 }
   
   // For print
   { format: 'png', quality: 0.95 }
   
   // For thumbnails
   { format: 'webp', quality: 0.7 }
   ```

3. **Crop before applying effects**
   ```javascript
   // More efficient
   const cropped = await eiip.cropImage(image, area);
   const withEffect = await eiip.applyEffect(cropped.canvas, 'sepia');
   ```

4. **Reuse instances for batch processing**
   ```javascript
   const effects = new EIIP.ImageEffects();
   for (const image of images) {
       await effects.applyEffect(image, 'grayscale');
   }
   ```

---

## Browser Compatibility

All new features work in modern browsers:
- Chrome 60+
- Firefox 55+
- Safari 11+
- Edge 79+

**Requirements:**
- Canvas API
- File API
- Promises/async-await

---

**EIIP v1.1.0** - More power, still lightweight! 🚀✨
