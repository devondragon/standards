# MACH Alliance, Open Data Model Utility Object: `Media`

## Table of contents
- [MACH Alliance, Open Data Model Utility Object: `Media`](#mach-alliance-open-data-model-utility-object-media)
  - [Table of contents](#table-of-contents)
  - [Entity purpose](#entity-purpose)
  - [Object: Media](#object-media)
  - [YAML Schema Definition](#yaml-schema-definition)
    - [Media Schema](#media-schema)
    - [Supporting Type Definitions](#supporting-type-definitions)
  - [Sample Object: Minimal Media](#sample-object-minimal-media)
  - [Sample Object: Product Image](#sample-object-product-image)
  - [Sample Object: Product Video](#sample-object-product-video)
  - [Sample Object: Document](#sample-object-document)
  - [Sample Object: Responsive Image Set](#sample-object-responsive-image-set)
  - [Sample Object: 3D Model](#sample-object-3d-model)
  - [Localization Pattern](#localization-pattern)
    - [Fields Supporting Localization](#fields-supporting-localization)
    - [Single Language](#single-language)
    - [Multi-Language](#multi-language)
  - [Implementation Guidelines](#implementation-guidelines)
    - [Media Types](#media-types)
    - [File Formats](#file-formats)
    - [URL Guidelines](#url-guidelines)
    - [Accessibility](#accessibility)
    - [Best Practices](#best-practices)
    - [Responsive Images](#responsive-images)
  - [Media Type Examples](#media-type-examples)
  - [Typical pitfalls](#typical-pitfalls)
    - [Asset Management Issues](#asset-management-issues)
    - [Field and Data Structure Problems](#field-and-data-structure-problems)
    - [Accessibility Failures](#accessibility-failures)
    - [Performance and Delivery](#performance-and-delivery)
    - [Integration Challenges](#integration-challenges)
    - [Security and Compliance](#security-and-compliance)
  - [Related Utility Objects](#related-utility-objects)

---

## Entity purpose

A standardized utility object for representing media assets across all entities in the MACH Alliance Common Data Model. This ensures consistent handling of images, videos, documents, and other digital assets across commerce engines, content management systems, digital asset management systems, and product information management systems.

The Media utility object provides:
- **Standardized media asset representation** across different platforms and services
- **Consistent URL and metadata handling** for reliable asset delivery
- **Support for multiple media types** including images, videos, documents, audio, and 3D models
- **Responsive image handling** with multiple resolutions and formats
- **Asset optimization and delivery** through CDN integration
- **Accessibility features** for inclusive commerce experiences
- **Rights and usage management** for copyright and licensing compliance
- **Cross-system references** for DAM, PIM, and CMS integration

---

## Object: Media

| Field                 | Description                                                              | Practice    |
| --------------------- | ------------------------------------------------------------------------ | ----------- |
| `id`                  | Unique identifier for the media asset                                    | SHOULD      |
| `type`                | Type of media asset (`image`, `video`, `document`, `audio`, `3d`)        | SHOULD      |
| `status`              | Lifecycle status (`draft`, `published`, `active`, `archived`, `deleted`) | SHOULD      |
| `external_references` | Dictionary of cross-system IDs (e.g., PIM, DAM, CMS)                     | SHOULD      |
| `created_at`          | ISO 8601 creation timestamp                                              | SHOULD      |
| `updated_at`          | ISO 8601 update timestamp                                                | SHOULD      |
| `title`               | Human-readable name or headline (string or localized object)             | SHOULD      |
| `description`         | Descriptive text about the asset's content (string or localized object)  | RECOMMENDED |
| `tags`                | Array of taxonomy keywords or labels                                     | COULD       |
| `file`                | Core file object with technical details                                  | MUST        |
| `variants`            | Array of alternative versions (sizes, formats, quality)                  | RECOMMENDED |
| `thumbnail`           | Reduced-size visual preview                                              | RECOMMENDED |
| `focal_point`         | Coordinates for intelligent cropping                                     | COULD       |
| `accessibility`       | Accessibility features including alt text and descriptions               | SHOULD      |
| `metadata`            | Technical metadata (EXIF, dimensions, duration)                          | COULD       |
| `extensions`          | Namespaced dictionary for extension data                                 | RECOMMENDED |


---

## YAML Schema Definition

### Media Schema

```yaml
Media:
  type: object
  required:
    - file
  properties:
    # Core identification
    id:
      type: string
      description: Unique identifier for the media asset
      # example: "MEDIA-IMG-001"

    type:
      type: string
      enum: ["image", "video", "document", "audio", "3d"]
      description: Type of media asset
      default: "image"

    status:
      type: string
      enum: ["draft", "published", "active", "archived", "deleted"]
      description: Lifecycle status of the asset
      default: "active"

    # External references
    external_references:
      type: object
      description: Dictionary of cross-system IDs
      additionalProperties:
        type: string
      # example:
      #   dam_id: "asset-1842356"
      #   pim_id: "pim-99871"
      #   cms_id: "entry-9d22bfa"
      #   youtube_id: "dQw4w9WgXcQ"

    # Timestamps
    created_at:
      type: string
      format: date-time
      description: ISO 8601 creation timestamp

    updated_at:
      type: string
      format: date-time
      description: ISO 8601 update timestamp

    # Display information (localizable)
    title:
      oneOf:
        - type: string  # Single language
        - type: object  # Multi-language
          additionalProperties:
            type: string
      description: Human-readable name or headline for the asset

    description:
      oneOf:
        - type: string  # Single language
        - type: object  # Multi-language
          additionalProperties:
            type: string
      description: Descriptive text about the asset's content and purpose

    # Categorization
    tags:
      type: array
      items:
        type: string
      description: Array of taxonomy keywords or labels
      # example: ["product", "hero", "lifestyle"]

    # File information
    file:
      $ref: "#/components/schemas/File"
      description: Core file object with technical details

    variants:
      type: array
      items:
        $ref: "#/components/schemas/MediaVariant"
      description: Array of alternative versions (sizes, formats, quality)

    thumbnail:
      $ref: "#/components/schemas/File"
      description: Reduced-size visual preview

    focal_point:
      $ref: "#/components/schemas/FocalPoint"
      description: Coordinates for intelligent cropping

    # Accessibility
    accessibility:
      $ref: "#/components/schemas/Accessibility"
      description: Accessibility features including alt text and descriptions

    # Technical metadata
    metadata:
      type: object
      description: Technical metadata from the file
      additionalProperties: true
      # example:
      #   exif:
      #     camera: "Canon EOS 5D Mark IV"
      #     lens: "24-70mm f/2.8"
      #     iso: 100
      #   color_profile: "sRGB"
      #   dpi: 300

    # Extensibility
    extensions:
      type: object
      description: Namespaced dictionary for extension data
      additionalProperties: true
      # example:
      #   rights:
      #     copyright: "© 2024 MACH Alliance"
      #     usage_terms: "Commercial use permitted"
      #     creator: "John Doe"
      #     expires: "2025-12-31"
```

### Supporting Type Definitions

```yaml
File:
  type: object
  required:
    - url
    - format
  properties:
    url:
      type: string
      format: uri
      description: URL to access the file
      # example: "https://cdn.example.com/assets/img-001987.webp"

    format:
      type: string
      description: File format/extension
      # example: "webp", "mp4", "pdf"

    size_bytes:
      type: integer
      description: File size in bytes
      minimum: 0
      # example: 1032840

    width:
      type: integer
      description: Width in pixels (for images/videos)
      minimum: 1

    height:
      type: integer
      description: Height in pixels (for images/videos)
      minimum: 1

    duration_seconds:
      type: number
      description: Duration in seconds (for video/audio)
      minimum: 0

    resolution:
      type: string
      description: Resolution description (e.g., "1920x1080", "4K")

    frame_rate:
      type: number
      description: Frames per second (for video)

    bitrate_kbps:
      type: integer
      description: Bitrate in kilobits per second (for video/audio)

    audio_channels:
      type: string
      enum: ["mono", "stereo", "5.1", "7.1"]
      description: Audio channel configuration

MediaVariant:
  type: object
  required:
    - url
    - format
    - variant_type
  properties:
    variant_type:
      type: string
      description: Type of variant (e.g., "responsive", "quality", "format")
      # example: "responsive"

    label:
      type: string
      description: Variant label (e.g., "small", "medium", "large", "webp", "avif")

    url:
      type: string
      format: uri
      description: URL to access the variant

    format:
      type: string
      description: File format of the variant

    size_bytes:
      type: integer
      description: File size in bytes

    width:
      type: integer
      description: Width in pixels

    height:
      type: integer
      description: Height in pixels

    quality:
      type: integer
      description: Quality percentage (1-100)
      minimum: 1
      maximum: 100

FocalPoint:
  type: object
  required:
    - x
    - y
  properties:
    x:
      type: number
      description: X coordinate (0.0 to 1.0, left to right)
      minimum: 0
      maximum: 1
      # example: 0.32

    y:
      type: number
      description: Y coordinate (0.0 to 1.0, top to bottom)
      minimum: 0
      maximum: 1
      # example: 0.35

    description:
      type: string
      description: Description of what the focal point represents
      # example: "Center of product logo"

Accessibility:
  type: object
  properties:
    alt_text:
      type: string
      description: Alternative text for screen readers
      # example: "Yellow MACH Alliance t-shirt with embroidered logo"

    long_description:
      type: string
      description: Extended description for complex images

    decorative:
      type: boolean
      description: Whether the image is purely decorative
      default: false

    transcript:
      type: string
      description: Text transcript (for video/audio)

    captions_url:
      type: string
      format: uri
      description: URL to caption file (for video)
```

---

## Sample Object: Minimal Media

Basic media object with only required fields.

```json
{
  "file": {
    "url": "https://cdn.example.com/assets/product.jpg",
    "format": "jpg"
  }
}
```

## Sample Object: Product Image

Product image with comprehensive metadata, responsive variants, and accessibility features.

```json
{
  "id": "MEDIA-IMG-001987",
  "type": "image",
  "status": "published",
  "external_references": {
    "pim_id": "pim-99871",
    "dam_id": "asset-1842356",
    "cms_id": "entry-9d22bfa"
  },
  "created_at": "2024-07-08T13:00:00Z",
  "updated_at": "2024-07-09T09:15:00Z",
  "title": "Yellow MACH Alliance T-Shirt Front View",
  "description": "Studio image of a yellow MACH Alliance t-shirt with a small embroidered logo on the left chest, photographed on a clean white background.",
  "tags": ["mach-alliance", "tshirt", "yellow", "apparel", "branded", "merchandise"],
  "file": {
    "url": "https://cdn.example.com/assets/img-001987.webp",
    "format": "webp",
    "size_bytes": 1032840,
    "width": 2400,
    "height": 1600
  },
  "variants": [
    {
      "variant_type": "responsive",
      "label": "small",
      "url": "https://cdn.example.com/assets/img-001987-sm.webp",
      "format": "webp",
      "size_bytes": 125000,
      "width": 600,
      "height": 400
    },
    {
      "variant_type": "responsive",
      "label": "medium",
      "url": "https://cdn.example.com/assets/img-001987-md.webp",
      "format": "webp",
      "size_bytes": 425000,
      "width": 1200,
      "height": 800
    },
    {
      "variant_type": "format",
      "label": "jpeg",
      "url": "https://cdn.example.com/assets/img-001987.jpg",
      "format": "jpg",
      "size_bytes": 1548000,
      "width": 2400,
      "height": 1600,
      "quality": 85
    }
  ],
  "thumbnail": {
    "url": "https://cdn.example.com/assets/img-001987-thumb.webp",
    "format": "webp",
    "size_bytes": 85240,
    "width": 480,
    "height": 320
  },
  "focal_point": {
    "x": 0.32,
    "y": 0.35,
    "description": "Center of the embroidered MACH Alliance logo on the left chest"
  },
  "accessibility": {
    "alt_text": "Yellow MACH Alliance t-shirt with a small embroidered logo on the chest",
    "long_description": "This is a high-resolution studio photograph of a bright yellow t-shirt, laid flat against a white background. The shirt features a subtle MACH Alliance embroidered logo in black, located on the left chest area. The fabric texture and stitching are clearly visible, and the lighting is balanced to emphasize detail without shadows.",
    "decorative": false
  },
  "metadata": {
    "exif": {
      "camera": "Canon EOS 5D Mark IV",
      "lens": "24-70mm f/2.8",
      "iso": 100,
      "aperture": "f/8",
      "shutter_speed": "1/125"
    },
    "color_profile": "sRGB",
    "dpi": 300
  },
  "extensions": {
    "rights": {
      "copyright": "© 2024 MACH Alliance",
      "usage_terms": "Unlimited commercial and editorial use. No expiration.",
      "creator": "Adam Peter Nielsen",
      "expires": null
    },
    "associations": {
      "product_ids": ["MACH-TSHIRT-YELLOW-2024"],
      "categories": ["merch", "apparel", "mach-alliance"]
    },
    "seo": {
      "keywords": ["mach alliance", "yellow t-shirt", "branded merchandise"],
      "schema_type": "ImageObject"
    }
  }
}
```





## Sample Object: Product Video

Video asset with multi-platform delivery, accessibility features, and comprehensive metadata.

```json
{
  "id": "MEDIA-VID-000561",
  "type": "video",
  "status": "active",
  "external_references": {
    "pim_id": "pim-vid-23391",
    "dam_id": "asset-vid-8090",
    "youtube_id": "dQw4w9WgXcQ",
    "cms_id": "entry-bv9d85dd"
  },
  "created_at": "2024-07-01T09:20:00Z",
  "updated_at": "2024-07-09T08:35:00Z",
  "title": "How to Assemble the MACH Alliance Trophy",
  "description": "Step-by-step tutorial video showing how to unpack and assemble the MACH Alliance trophy.",
  "tags": ["assembly", "mach-alliance", "tutorial", "how-to", "product-guide"],
  "file": {
    "url": "https://cdn.example.com/assets/vid-000561.mp4",
    "format": "mp4",
    "size_bytes": 104328304,
    "duration_seconds": 148,
    "width": 1920,
    "height": 1080,
    "resolution": "1920x1080",
    "frame_rate": 30,
    "bitrate_kbps": 4500,
    "audio_channels": "stereo"
  },
  "variants": [
    {
      "variant_type": "quality",
      "label": "720p",
      "url": "https://cdn.example.com/assets/vid-000561-720p.mp4",
      "format": "mp4",
      "size_bytes": 52164152,
      "width": 1280,
      "height": 720,
      "bitrate_kbps": 2500
    },
    {
      "variant_type": "quality",
      "label": "480p",
      "url": "https://cdn.example.com/assets/vid-000561-480p.mp4",
      "format": "mp4",
      "size_bytes": 31298592,
      "width": 854,
      "height": 480,
      "bitrate_kbps": 1500
    },
    {
      "variant_type": "format",
      "label": "webm",
      "url": "https://cdn.example.com/assets/vid-000561.webm",
      "format": "webm",
      "size_bytes": 89280000,
      "width": 1920,
      "height": 1080
    }
  ],
  "thumbnail": {
    "url": "https://cdn.example.com/assets/thumbnails/vid-000561-thumb.webp",
    "format": "webp",
    "size_bytes": 78412,
    "width": 640,
    "height": 360
  },
  "accessibility": {
    "alt_text": "Video tutorial showing MACH Alliance trophy assembly",
    "transcript": "Full transcript available at /transcripts/vid-000561.txt",
    "captions_url": "https://cdn.example.com/captions/vid-000561.vtt",
    "decorative": false
  },
  "extensions": {
    "rights": {
      "copyright": "© 2024 MACH Alliance",
      "usage_terms": "Unlimited commercial use across owned and third-party platforms. Attribution not required.",
      "expires": null
    },
    "associations": {
      "product_ids": ["MACH-TROPHY-2024"],
      "categories": ["tutorials", "support", "video-content"]
    },
    "playback": {
      "autoplay": false,
      "default_playback_speed": 1.0,
      "available_speeds": [0.5, 0.75, 1.0, 1.25, 1.5, 2.0],
      "chapters": [
        {"title": "Introduction", "start_time": 0},
        {"title": "Unboxing", "start_time": 15},
        {"title": "Assembly Steps", "start_time": 45},
        {"title": "Final Result", "start_time": 120}
      ],
      "platforms": {
        "youtube": "https://www.youtube.com/watch?v=dQw4w9WgXcQ",
        "vimeo": "https://vimeo.com/123456789"
      }
    }
  }
}
```

## Sample Object: Document

PDF document with metadata and access control.

```json
{
  "id": "MEDIA-DOC-001",
  "type": "document",
  "status": "published",
  "external_references": {
    "dam_id": "doc-asset-789",
    "cms_id": "doc-entry-456"
  },
  "created_at": "2024-06-15T10:00:00Z",
  "updated_at": "2024-06-15T10:00:00Z",
  "title": {
    "en-US": "Product Assembly Instructions",
    "es-ES": "Instrucciones de Montaje del Producto",
    "fr-FR": "Instructions de Montage du Produit"
  },
  "description": {
    "en-US": "Detailed assembly instructions with diagrams",
    "es-ES": "Instrucciones detalladas de montaje con diagramas",
    "fr-FR": "Instructions de montage détaillées avec schémas"
  },
  "tags": ["manual", "assembly", "instructions", "pdf"],
  "file": {
    "url": "https://cdn.example.com/docs/assembly-instructions.pdf",
    "format": "pdf",
    "size_bytes": 2457600
  },
  "thumbnail": {
    "url": "https://cdn.example.com/docs/assembly-instructions-thumb.jpg",
    "format": "jpg",
    "size_bytes": 45000,
    "width": 200,
    "height": 283
  },
  "metadata": {
    "pages": 12,
    "language": "en-US",
    "searchable": true,
    "version": "2.1"
  },
  "extensions": {
    "rights": {
      "copyright": "© 2024 MACH Alliance",
      "usage_terms": "Internal use only"
    },
    "associations": {
      "product_ids": ["PROD-001", "PROD-002"],
      "categories": ["documentation", "support"]
    },
    "access": {
      "requires_authentication": false,
      "allowed_regions": ["US", "CA", "EU"],
      "download_enabled": true
    }
  }
}
```

## Sample Object: Responsive Image Set

Image with multiple responsive variants for different screen sizes and devices.

```json
{
  "id": "MEDIA-IMG-HERO-001",
  "type": "image",
  "status": "active",
  "external_references": {
    "dam_id": "hero-asset-123"
  },
  "created_at": "2024-07-20T14:00:00Z",
  "updated_at": "2024-07-20T14:00:00Z",
  "title": "Homepage Hero Banner - Summer Sale",
  "tags": ["hero", "banner", "campaign", "summer-sale"],
  "file": {
    "url": "https://cdn.example.com/images/hero-summer-2400.webp",
    "format": "webp",
    "size_bytes": 845000,
    "width": 2400,
    "height": 800
  },
  "variants": [
    {
      "variant_type": "responsive",
      "label": "mobile",
      "url": "https://cdn.example.com/images/hero-summer-640.webp",
      "format": "webp",
      "size_bytes": 125000,
      "width": 640,
      "height": 480
    },
    {
      "variant_type": "responsive",
      "label": "tablet",
      "url": "https://cdn.example.com/images/hero-summer-1024.webp",
      "format": "webp",
      "size_bytes": 285000,
      "width": 1024,
      "height": 600
    },
    {
      "variant_type": "responsive",
      "label": "desktop",
      "url": "https://cdn.example.com/images/hero-summer-1920.webp",
      "format": "webp",
      "size_bytes": 545000,
      "width": 1920,
      "height": 700
    },
    {
      "variant_type": "format",
      "label": "avif",
      "url": "https://cdn.example.com/images/hero-summer-2400.avif",
      "format": "avif",
      "size_bytes": 425000,
      "width": 2400,
      "height": 800
    }
  ],
  "focal_point": {
    "x": 0.65,
    "y": 0.4,
    "description": "Main product display area"
  },
  "accessibility": {
    "alt_text": "Summer sale banner featuring 50% off on selected items",
    "decorative": false
  },
  "extensions": {
    "responsive": {
      "art_direction": {
        "mobile": {
          "crop": "center",
          "aspect_ratio": "4:3"
        },
        "desktop": {
          "crop": "full",
          "aspect_ratio": "3:1"
        }
      },
      "loading_strategy": "lazy",
      "sizes": "(max-width: 640px) 100vw, (max-width: 1024px) 100vw, 1920px"
    },
    "campaign": {
      "campaign_id": "SUMMER-SALE-2024",
      "start_date": "2024-07-01",
      "end_date": "2024-08-31",
      "cta_text": "Shop Now",
      "cta_url": "/sale/summer"
    }
  }
}
```

## Sample Object: 3D Model

3D model asset for AR/VR experiences and product visualization.

```json
{
  "id": "MEDIA-3D-001",
  "type": "3d",
  "status": "active",
  "external_references": {
    "dam_id": "3d-asset-456",
    "pim_id": "pim-3d-789"
  },
  "created_at": "2024-08-01T11:00:00Z",
  "updated_at": "2024-08-01T11:00:00Z",
  "title": "MACH Alliance Trophy 3D Model",
  "description": "Interactive 3D model of the MACH Alliance trophy for AR viewing",
  "tags": ["3d", "ar", "product", "trophy", "interactive"],
  "file": {
    "url": "https://cdn.example.com/3d/mach-trophy.glb",
    "format": "glb",
    "size_bytes": 15728640
  },
  "variants": [
    {
      "variant_type": "quality",
      "label": "low",
      "url": "https://cdn.example.com/3d/mach-trophy-low.glb",
      "format": "glb",
      "size_bytes": 3145728
    },
    {
      "variant_type": "format",
      "label": "usdz",
      "url": "https://cdn.example.com/3d/mach-trophy.usdz",
      "format": "usdz",
      "size_bytes": 18874368
    }
  ],
  "thumbnail": {
    "url": "https://cdn.example.com/3d/mach-trophy-preview.jpg",
    "format": "jpg",
    "size_bytes": 125000,
    "width": 800,
    "height": 800
  },
  "metadata": {
    "vertices": 125840,
    "faces": 251680,
    "materials": 3,
    "textures": 5,
    "animations": 1,
    "bounding_box": {
      "width": 0.3,
      "height": 0.45,
      "depth": 0.3,
      "unit": "meters"
    }
  },
  "extensions": {
    "ar": {
      "supports_ar_quicklook": true,
      "supports_webxr": true,
      "scale": "auto",
      "placement": "floor",
      "min_ios_version": "12.0",
      "min_android_version": "7.0"
    },
    "rendering": {
      "pbr_materials": true,
      "environment_map": "studio_lighting",
      "shadow_intensity": 0.8,
      "exposure": 1.2
    },
    "associations": {
      "product_ids": ["MACH-TROPHY-2024"],
      "categories": ["awards", "3d-models"]
    }
  }
}
```

---

## Localization Pattern

The Media utility object supports localization for text fields that may need translation.

### Fields Supporting Localization
- `title` - Media title or name
- `description` - Media description

### Single Language
```json
{
  "title": "Product Image",
  "description": "High-quality product photograph"
}
```

### Multi-Language
```json
{
  "title": {
    "en-US": "Product Image",
    "es-ES": "Imagen del Producto",
    "fr-FR": "Image du Produit"
  },
  "description": {
    "en-US": "High-quality product photograph",
    "es-ES": "Fotografía de producto de alta calidad",
    "fr-FR": "Photographie de produit haute qualité"
  }
}
```

---

## Implementation Guidelines

### Media Types
- **image**: Photos, graphics, icons
- **video**: Video files, animations
- **document**: PDFs, documents, specifications
- **audio**: Sound files, podcasts
- **3d**: 3D models, AR assets

### File Formats
- **Images**: jpg, jpeg, png, gif, webp, svg
- **Videos**: mp4, webm, avi, mov
- **Documents**: pdf, doc, docx, txt
- **Audio**: mp3, wav, aac, ogg

### URL Guidelines
- Use HTTPS URLs for security
- Consider CDN URLs for performance
- Support multiple image sizes/resolutions
- Include versioning for cache busting

### Accessibility
- Always include meaningful `alt` text
- Use descriptive `label` fields
- Consider screen reader compatibility
- Provide alternative formats when needed

### Best Practices
- Always include `url`, `alt`, and `type` fields
- Use consistent naming conventions for labels
- Optimize images for web delivery
- Consider lazy loading for performance
- Implement proper error handling for broken URLs

### Responsive Images
- Provide multiple sizes for different devices
- Use appropriate formats (WebP for modern browsers)
- Consider aspect ratios for different layouts
- Implement proper fallbacks

---

## Media Type Examples

| Type     | Common Formats            | Use Cases                                   |
| -------- | ------------------------- | ------------------------------------------- |
| image    | jpg, png, webp, svg, avif | Product photos, banners, icons, logos       |
| video    | mp4, webm, mov, avi       | Product demos, tutorials, ads, testimonials |
| document | pdf, doc, docx, txt       | Size charts, manuals, specs, certificates   |
| audio    | mp3, wav, aac, ogg        | Product sounds, podcasts, voice guides      |
| 3d       | glb, gltf, usdz, obj      | AR/VR experiences, 3D models, configurators |

---

## Typical pitfalls

### Asset Management Issues
- **Missing CDN integration** - Serving media directly from application servers causes performance bottlenecks
- **No image optimization** - Serving full-resolution images to all devices wastes bandwidth
- **Ignoring modern formats** - Not supporting WebP, AVIF reduces performance and increases costs
- **No responsive variants** - Single image size doesn't work across devices
- **Missing focal point data** - Automated cropping cuts off important content

### Field and Data Structure Problems
- **Using external_references instead of external_references** - Breaks consistency with other entities
- **Storing URLs in multiple fields** - Creates confusion about canonical source
- **Missing file metadata** - No size, dimensions, or format information makes optimization difficult
- **Flat structure for variants** - Makes it hard to manage multiple versions
- **No localization support** - Can't provide language-specific media titles/descriptions

### Accessibility Failures
- **Missing alt text** - Makes content inaccessible to screen readers
- **No video captions or transcripts** - Excludes hearing-impaired users
- **Marking content images as decorative** - Removes important context
- **Generic alt text** - "Image" or "Photo" provides no value
- **Missing long descriptions** - Complex diagrams need detailed explanations

### Performance and Delivery
- **No lazy loading strategy** - Loading all images impacts initial page load
- **Missing image dimensions** - Causes layout shift during loading
- **No quality variants** - Can't optimize for bandwidth-constrained users
- **Serving wrong formats** - Using PNG for photos, JPEG for graphics
- **No caching headers** - Forces re-download of unchanged assets

### Integration Challenges
- **No cross-system references** - Can't sync assets between DAM, PIM, CMS
- **Missing usage rights metadata** - Legal compliance issues
- **No asset versioning** - Can't track changes or rollback
- **Tight coupling to specific CDN** - Hard to change providers
- **No batch operations support** - Inefficient for bulk updates

### Security and Compliance
- **Exposing internal paths** - Security risk in URLs
- **No access control** - Can't restrict document downloads
- **Missing expiration handling** - Licensed content used beyond terms
- **No watermarking support** - Can't protect premium content
- **Storing sensitive data in public CDN** - Privacy violations

---

## Related Utility Objects

- **Product**: Product information with media gallery
- **Category**: Category images and banners
- **Campaign**: Campaign creative assets
- **SEO**: Media metadata for search optimization
- **Language**: Localization support for media content

---

> This MACH Alliance Canonical Data Model is intentionally __vendor-neutral__ and serves as a foundation for interoperability across composable architectures. It is __continually evolving__ through community contributions, which are reviewed and approved collaboratively.
>
> All contributions are made under the __Creative Commons Attribution 4.0 International License (CC BY 4.0)__. By submitting a contribution, you agree to license your content under <a href="https://creativecommons.org/licenses/by/4.0/deed.en">CC BY 4.0</a>, allowing others to share and adapt the material with proper attribution.
