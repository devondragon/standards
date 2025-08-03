# MACH Alliance, Open Data Model Utility Object: `Language`

## Table of contents

- [MACH Alliance, Open Data Model Utility Object: `Language`](#mach-alliance-open-data-model-utility-object-language)
  - [Table of contents](#table-of-contents)
  - [Entity purpose](#entity-purpose)
    - [Key Use Cases](#key-use-cases)
  - [Object: Language](#object-language)
  - [YAML Schema Definition](#yaml-schema-definition)
    - [Language Schema](#language-schema)
    - [Supporting Type Definitions](#supporting-type-definitions)
  - [Sample Object: Minimal Language](#sample-object-minimal-language)
  - [Sample Objects](#sample-objects)
    - [Sample Object: English (US)](#sample-object-english-us)
    - [Sample Object: Spanish (Mexico)](#sample-object-spanish-mexico)
    - [Sample Object: Arabic (Saudi Arabia)](#sample-object-arabic-saudi-arabia)
    - [Sample Object: Chinese (Simplified)](#sample-object-chinese-simplified)
    - [Sample Object: English (UK)](#sample-object-english-uk)
  - [Implementation Guidelines](#implementation-guidelines)
    - [Language Codes](#language-codes)
    - [Locale Structure](#locale-structure)
    - [Fallback Strategy](#fallback-strategy)
    - [Formatting Standards](#formatting-standards)
  - [Common Language Codes](#common-language-codes)
    - [Primary Languages](#primary-languages)
    - [Common Locales](#common-locales)
  - [Typical pitfalls](#typical-pitfalls)
    - [Language Code Standards](#language-code-standards)
    - [Localization and Formatting](#localization-and-formatting)
    - [Fallback and Coverage](#fallback-and-coverage)
    - [Regional Variations](#regional-variations)
    - [Integration Issues](#integration-issues)
    - [Typography and Display](#typography-and-display)
  - [Related Utility Objects](#related-utility-objects)

---

## Entity purpose
The Language utility object provides a standardized representation for languages, locales, and regional variations. It supports internationalization (i18n) and localization (l10n) across multiple domains including eCommerce, content management, and user interfaces.

### Key Use Cases
- Multi-language content and product information
- Regional language variations (e.g., en-US vs en-GB)
- Locale-specific formatting (dates, numbers, currency)
- Language preferences and fallbacks
- SEO and content localization
- User interface language selection

---

## Object: Language

| Field                 | Description                                                           | Practice    |
| --------------------- | --------------------------------------------------------------------- | ----------- |
| `code`                | ISO 639-1 language code (e.g., "en", "es", "fr")                      | MUST        |
| `name`                | Language name in the language itself (string or localized object)     | MUST        |
| `locale`              | Full locale identifier (e.g., "en-US", "es-MX")                       | MUST        |
| `region`              | ISO 3166-1 alpha-2 region/country code                                | SHOULD      |
| `script`              | ISO 15924 script code (e.g., "Latn", "Cyrl", "Arab", "Hans")          | COULD       |
| `direction`           | Text direction (`ltr`, `rtl`)                                         | SHOULD      |
| `status`              | Language status (`active`, `inactive`, `deprecated`, `experimental`)  | SHOULD      |
| `external_references` | Dictionary of cross-system IDs (e.g., ISO codes, translation systems) | SHOULD      |
| `created_at`          | ISO 8601 creation timestamp                                           | SHOULD      |
| `updated_at`          | ISO 8601 update timestamp                                             | SHOULD      |
| `formatting`          | Locale-specific formatting rules                                      | RECOMMENDED |
| `fallback_locales`    | Ordered array of fallback locales                                     | RECOMMENDED |
| `extensions`          | Namespaced dictionary for extension data                              | RECOMMENDED |

---

## YAML Schema Definition

### Language Schema

```yaml
Language:
  type: object
  required:
    - code
    - name
    - locale
  properties:
    # Core identification
    code:
      type: string
      pattern: "^[a-z]{2,3}$"
      description: ISO 639-1 or ISO 639-2 language code
      # example: "en", "es", "zh"

    name:
      oneOf:
        - type: string  # Single language
        - type: object  # Multi-language
          additionalProperties:
            type: string
      description: Language name in the language itself
      # example: "English" or {"en": "English", "es": "Inglés"}

    locale:
      type: string
      pattern: "^[a-z]{2,3}(-[A-Z][a-z]{3})?(-[A-Z]{2})?$"
      description: Full locale identifier (language-script-region)
      # example: "en-US", "zh-Hans-CN"

    # Regional and script information
    region:
      type: string
      pattern: "^[A-Z]{2}$"
      description: ISO 3166-1 alpha-2 region/country code
      # example: "US", "GB", "CN"

    script:
      type: string
      pattern: "^[A-Z][a-z]{3}$"
      description: ISO 15924 script code
      # example: "Latn", "Arab", "Hans", "Hant"

    direction:
      type: string
      enum: ["ltr", "rtl"]
      description: Text direction
      default: "ltr"

    # Status and lifecycle
    status:
      type: string
      enum: ["active", "inactive", "deprecated", "experimental"]
      description: Language availability status
      default: "active"

    # External references
    external_references:
      type: object
      description: Dictionary of cross-system IDs
      additionalProperties:
        type: string
      # example:
      #   iso_639_1: "en"
      #   iso_639_2: "eng"
      #   iso_639_3: "eng"
      #   bcp_47: "en-US"
      #   translation_system: "lang-en-us-123"

    # Timestamps
    created_at:
      type: string
      format: date-time
      description: ISO 8601 creation timestamp

    updated_at:
      type: string
      format: date-time
      description: ISO 8601 update timestamp

    # Formatting and localization
    formatting:
      $ref: "#/components/schemas/LocaleFormatting"
      description: Locale-specific formatting rules

    fallback_locales:
      type: array
      items:
        type: string
      description: Ordered array of fallback locales
      # example: ["es", "en"]

    # Extensibility
    extensions:
      type: object
      description: Namespaced dictionary for extension data
      additionalProperties: true
      # example:
      #   seo:
      #     hreflang: "en-US"
      #   translation:
      #     coverage: 0.95
      #     last_updated: "2024-07-01"
```

### Supporting Type Definitions

```yaml
LocaleFormatting:
  type: object
  properties:
    date_format:
      type: string
      description: Date format pattern
      # example: "MM/dd/yyyy" or "dd/MM/yyyy"

    time_format:
      type: string
      description: Time format pattern
      # example: "h:mm a" or "HH:mm"

    date_time_format:
      type: string
      description: Combined date-time format pattern

    first_day_of_week:
      type: integer
      description: First day of week (0=Sunday, 1=Monday)
      minimum: 0
      maximum: 6

    number_format:
      $ref: "#/components/schemas/NumberFormatting"
      description: Number formatting rules

    currency_format:
      $ref: "#/components/schemas/CurrencyFormatting"
      description: Currency formatting rules

    measurement_system:
      type: string
      enum: ["metric", "imperial"]
      description: Preferred measurement system

NumberFormatting:
  type: object
  properties:
    decimal_separator:
      type: string
      description: Decimal separator character
      # example: "." or ","

    thousands_separator:
      type: string
      description: Thousands separator character
      # example: "," or "."

    decimal_places:
      type: integer
      description: Default decimal places
      minimum: 0
      default: 2

    grouping:
      type: array
      items:
        type: integer
      description: Digit grouping pattern
      # example: [3] for 1,000,000 or [3, 2] for 1,00,00,000

CurrencyFormatting:
  type: object
  properties:
    symbol:
      type: string
      description: Default currency symbol
      # example: "$", "€", "£"

    code:
      type: string
      pattern: "^[A-Z]{3}$"
      description: Default ISO 4217 currency code
      # example: "USD", "EUR", "GBP"

    position:
      type: string
      enum: ["before", "after"]
      description: Symbol position relative to amount
      default: "before"

    spacing:
      type: boolean
      description: Space between symbol and amount
      default: false

    decimal_places:
      type: integer
      description: Currency-specific decimal places
      minimum: 0
      default: 2
```

---

## Sample Object: Minimal Language

Basic language object with only required fields.

```json
{
  "code": "en",
  "name": "English",
  "locale": "en-US"
}
```

## Sample Objects

### Sample Object: English (US)

Complete language configuration for US English with formatting rules.

```json
{
  "code": "en",
  "name": "English",
  "locale": "en-US",
  "region": "US",
  "script": "Latn",
  "direction": "ltr",
  "status": "active",
  "external_references": {
    "iso_639_1": "en",
    "iso_639_2": "eng",
    "iso_639_3": "eng",
    "iso_3166": "US",
    "bcp_47": "en-US"
  },
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "formatting": {
    "date_format": "MM/dd/yyyy",
    "time_format": "h:mm a",
    "date_time_format": "MM/dd/yyyy h:mm a",
    "first_day_of_week": 0,
    "number_format": {
      "decimal_separator": ".",
      "thousands_separator": ",",
      "decimal_places": 2,
      "grouping": [3]
    },
    "currency_format": {
      "symbol": "$",
      "code": "USD",
      "position": "before",
      "spacing": false,
      "decimal_places": 2
    },
    "measurement_system": "imperial"
  },
  "fallback_locales": ["en"],
  "extensions": {
    "seo": {
      "hreflang": "en-US",
      "canonical_locale": true
    },
    "translation": {
      "coverage": 1.0,
      "is_source": true,
      "last_updated": "2024-01-01"
    },
    "analytics": {
      "language_code": "en",
      "region_code": "US",
      "usage_rank": 1
    }
  }
}
```

### Sample Object: Spanish (Mexico)

Mexican Spanish with region-specific formatting.

```json
{
  "code": "es",
  "name": "Español",
  "locale": "es-MX",
  "region": "MX",
  "script": "Latn",
  "direction": "ltr",
  "status": "active",
  "external_references": {
    "iso_639_1": "es",
    "iso_639_2": "spa",
    "iso_3166": "MX",
    "bcp_47": "es-MX"
  },
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "formatting": {
    "date_format": "dd/MM/yyyy",
    "time_format": "HH:mm",
    "date_time_format": "dd/MM/yyyy HH:mm",
    "first_day_of_week": 1,
    "number_format": {
      "decimal_separator": ".",
      "thousands_separator": ",",
      "decimal_places": 2,
      "grouping": [3]
    },
    "currency_format": {
      "symbol": "$",
      "code": "MXN",
      "position": "before",
      "spacing": true,
      "decimal_places": 2
    },
    "measurement_system": "metric"
  },
  "fallback_locales": ["es", "en"],
  "extensions": {
    "seo": {
      "hreflang": "es-MX",
      "alternate_codes": ["es-419"]
    },
    "translation": {
      "coverage": 0.98,
      "parent_locale": "es",
      "last_updated": "2024-06-15"
    },
    "regional": {
      "date_notation": "little-endian",
      "decimal_notation": "comma-decimal",
      "phone_format": "+52 (###) ###-####"
    }
  }
}
```

### Sample Object: Arabic (Saudi Arabia)

Arabic language with RTL direction and regional formatting.

```json
{
  "code": "ar",
  "name": {
    "ar": "العربية",
    "en": "Arabic"
  },
  "locale": "ar-SA",
  "region": "SA",
  "script": "Arab",
  "direction": "rtl",
  "status": "active",
  "external_references": {
    "iso_639_1": "ar",
    "iso_639_2": "ara",
    "iso_639_3": "arb",
    "iso_3166": "SA",
    "bcp_47": "ar-SA"
  },
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "formatting": {
    "date_format": "dd/MM/yyyy",
    "time_format": "HH:mm",
    "date_time_format": "dd/MM/yyyy HH:mm",
    "first_day_of_week": 6,
    "number_format": {
      "decimal_separator": ".",
      "thousands_separator": ",",
      "decimal_places": 2,
      "grouping": [3]
    },
    "currency_format": {
      "symbol": "ر.س",
      "code": "SAR",
      "position": "after",
      "spacing": true,
      "decimal_places": 2
    },
    "measurement_system": "metric"
  },
  "fallback_locales": ["ar", "en"],
  "extensions": {
    "seo": {
      "hreflang": "ar-SA",
      "canonical_arabic": true
    },
    "translation": {
      "coverage": 0.95,
      "parent_locale": "ar",
      "last_updated": "2024-06-01"
    },
    "calendar": {
      "primary": "gregorian",
      "secondary": "islamic-umalqura",
      "weekend_days": [5, 6]
    },
    "typography": {
      "font_family": "Noto Sans Arabic",
      "line_height_multiplier": 1.5,
      "requires_shaping": true
    }
  }
}
```

### Sample Object: Chinese (Simplified)

Simplified Chinese for mainland China.

```json
{
  "code": "zh",
  "name": {
    "zh": "中文",
    "en": "Chinese"
  },
  "locale": "zh-CN",
  "region": "CN",
  "script": "Hans",
  "direction": "ltr",
  "status": "active",
  "external_references": {
    "iso_639_1": "zh",
    "iso_639_2": "zho",
    "iso_639_3": "cmn",
    "iso_3166": "CN",
    "bcp_47": "zh-Hans-CN"
  },
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "formatting": {
    "date_format": "yyyy-MM-dd",
    "time_format": "HH:mm",
    "date_time_format": "yyyy-MM-dd HH:mm",
    "first_day_of_week": 1,
    "number_format": {
      "decimal_separator": ".",
      "thousands_separator": ",",
      "decimal_places": 2,
      "grouping": [3]
    },
    "currency_format": {
      "symbol": "¥",
      "code": "CNY",
      "position": "before",
      "spacing": false,
      "decimal_places": 2
    },
    "measurement_system": "metric"
  },
  "fallback_locales": ["zh-TW", "en"],
  "extensions": {
    "seo": {
      "hreflang": "zh-CN",
      "alternate_tags": ["zh-Hans"]
    },
    "translation": {
      "coverage": 0.99,
      "is_simplified": true,
      "last_updated": "2024-07-01"
    },
    "input_method": {
      "primary": "pinyin",
      "alternatives": ["wubi", "stroke"]
    },
    "typography": {
      "font_family": "Noto Sans CJK SC",
      "line_height_multiplier": 1.4,
      "character_width": "fullwidth"
    }
  }
}
```

### Sample Object: English (UK)

British English with UK-specific formatting.

```json
{
  "code": "en",
  "name": "English",
  "locale": "en-GB",
  "region": "GB",
  "script": "Latn",
  "direction": "ltr",
  "status": "active",
  "external_references": {
    "iso_639_1": "en",
    "iso_639_2": "eng",
    "iso_3166": "GB",
    "bcp_47": "en-GB"
  },
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "formatting": {
    "date_format": "dd/MM/yyyy",
    "time_format": "HH:mm",
    "date_time_format": "dd/MM/yyyy HH:mm",
    "first_day_of_week": 1,
    "number_format": {
      "decimal_separator": ".",
      "thousands_separator": ",",
      "decimal_places": 2,
      "grouping": [3]
    },
    "currency_format": {
      "symbol": "£",
      "code": "GBP",
      "position": "before",
      "spacing": false,
      "decimal_places": 2
    },
    "measurement_system": "metric"
  },
  "fallback_locales": ["en-US", "en"],
  "extensions": {
    "seo": {
      "hreflang": "en-GB",
      "priority_region": "europe"
    },
    "translation": {
      "coverage": 1.0,
      "variant": "british",
      "last_updated": "2024-01-01"
    },
    "regional": {
      "spelling": "british",
      "vocabulary": "british",
      "postal_code_format": "[A-Z]{1,2}[0-9]{1,2} [0-9][A-Z]{2}"
    }
  }
}
```

---

## Implementation Guidelines

### Language Codes
- Use ISO 639-1 for primary language codes (2-letter codes)
- Use ISO 639-2 for extended language codes when needed (3-letter codes)
- Use ISO 3166-1 alpha-2 for region codes
- Use ISO 15924 for script codes

### Locale Structure
- Format: `language-REGION` (e.g., "en-US", "es-MX")
- Include script when needed: `language-Script-REGION` (e.g., "zh-Hans-CN")
- Use lowercase for language, uppercase for region

### Fallback Strategy
- Define fallback languages for incomplete translations
- Support multiple fallback levels (e.g., es-MX → es → en)
- Include fallback information in extensions

### Formatting Standards
- Follow locale-specific formatting conventions
- Support both standard and custom formatting patterns
- Include currency formatting for e-commerce contexts

---

## Common Language Codes

### Primary Languages

| Code | Name       | Native Name | Direction |
| ---- | ---------- | ----------- | --------- |
| `en` | English    | English     | ltr       |
| `es` | Spanish    | Español     | ltr       |
| `fr` | French     | Français    | ltr       |
| `de` | German     | Deutsch     | ltr       |
| `it` | Italian    | Italiano    | ltr       |
| `pt` | Portuguese | Português   | ltr       |
| `ru` | Russian    | Русский     | ltr       |
| `zh` | Chinese    | 中文        | ltr       |
| `ja` | Japanese   | 日本語      | ltr       |
| `ko` | Korean     | 한국어      | ltr       |
| `ar` | Arabic     | العربية     | rtl       |
| `hi` | Hindi      | हिन्दी         | ltr       |

### Common Locales

| Locale  | Language | Region         | Description         |
| ------- | -------- | -------------- | ------------------- |
| `en-US` | English  | United States  | US English          |
| `en-GB` | English  | United Kingdom | British English     |
| `en-CA` | English  | Canada         | Canadian English    |
| `es-ES` | Spanish  | Spain          | European Spanish    |
| `es-MX` | Spanish  | Mexico         | Mexican Spanish     |
| `fr-FR` | French   | France         | European French     |
| `fr-CA` | French   | Canada         | Canadian French     |
| `de-DE` | German   | Germany        | German              |
| `zh-CN` | Chinese  | China          | Simplified Chinese  |
| `zh-TW` | Chinese  | Taiwan         | Traditional Chinese |
| `ja-JP` | Japanese | Japan          | Japanese            |
| `ko-KR` | Korean   | South Korea    | Korean              |

---

## Typical pitfalls

### Language Code Standards
- **Inconsistent language codes** - Always use ISO 639-1 (2-letter) or ISO 639-2 (3-letter) standards
- **Wrong locale format** - Use proper BCP 47 format: language-script-region (e.g., zh-Hans-CN)
- **Missing region codes** - Use ISO 3166-1 alpha-2 for regions, not custom abbreviations
- **Incorrect script codes** - Use ISO 15924 four-letter codes (e.g., Latn, Arab, Hans)
- **Case sensitivity errors** - Language codes lowercase, region codes uppercase

### Localization and Formatting
- **Missing direction information** - Always specify text direction (ltr/rtl) for proper UI rendering
- **Incomplete formatting rules** - Include all locale-specific formatting (dates, numbers, currency)
- **Wrong date patterns** - Use locale-appropriate date formats (MM/dd/yyyy vs dd/MM/yyyy)
- **Missing first day of week** - Different cultures start weeks on different days
- **Incorrect decimal separators** - Many locales use comma instead of period

### Fallback and Coverage
- **No fallback strategy** - Define clear fallback chains for incomplete translations
- **Circular fallbacks** - Avoid loops in fallback chains (e.g., es-MX → es → es-MX)
- **Missing parent locale** - Regional variants should fallback to parent (es-MX → es)
- **No translation coverage tracking** - Track percentage of translated content
- **English-only fallback** - Consider regional fallbacks before defaulting to English

### Regional Variations
- **Ignoring regional differences** - US vs UK English, European vs Latin American Spanish
- **Wrong currency assumptions** - Don't assume currency from language (en-US → USD, en-CA → CAD)
- **Missing measurement systems** - Track metric vs imperial preferences by locale
- **Incorrect phone formats** - Phone number formats vary by country
- **Calendar assumptions** - Different regions use different calendar systems and weekends

### Integration Issues
- **Using external_references instead of external_references** - Breaks consistency with other entities
- **Missing system references** - Include references to translation management systems
- **No timestamp tracking** - Track when languages were added or updated
- **Hardcoded text directions** - Derive from script/language instead of hardcoding
- **Missing SEO hreflang tags** - Critical for multilingual SEO

### Typography and Display
- **Font assumptions** - Different scripts need different fonts (Arabic, CJK, etc.)
- **Line height issues** - Some scripts need more vertical space
- **Character width ignorance** - CJK characters are full-width
- **Missing text shaping** - Arabic and Indic scripts require shaping
- **No input method info** - Document preferred input methods for CJK languages

---

## Related Utility Objects

- **Currency**: Currency formatting tied to locale preferences
- **Address**: Address formats vary by locale
- **SEO**: Hreflang tags for multilingual sites
- **Media**: Localized media titles and descriptions
- **Product**: Multi-language product information

---

> This MACH Alliance Canonical Data Model is intentionally __vendor-neutral__ and serves as a foundation for interoperability across composable architectures. It is __continually evolving__ through community contributions, which are reviewed and approved collaboratively.
>
> All contributions are made under the __Creative Commons Attribution 4.0 International License (CC BY 4.0)__. By submitting a contribution, you agree to license your content under <a href="https://creativecommons.org/licenses/by/4.0/deed.en">CC BY 4.0</a>, allowing others to share and adapt the material with proper attribution.

