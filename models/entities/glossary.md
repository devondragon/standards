# MACH Alliance Common Data Model - Field Glossary

## Table of Contents

- [Core Identification Fields](#core-identification-fields)
- [Lifecycle and Status Fields](#lifecycle-and-status-fields)
- [Time-related Fields](#time-related-fields)
- [Localization Fields](#localization-fields)
- [Integration Fields](#integration-fields)
- [Financial Fields](#financial-fields)
- [Relationship Fields](#relationship-fields)
- [Content and Media Fields](#content-and-media-fields)
- [Extensibility Fields](#extensibility-fields)
- [Data Quality Fields](#data-quality-fields)

---

## Core Identification Fields

### `id`
**Purpose**: Unique identifier for the entity within its given context  
**Type**: string  
**Practice**: MUST/SHOULD  
**Examples**: `"PROD-001"`, `"ORDER-123"`, `"CUST-456"`  
**Used in**: All entities  
**Notes**: Primary key for entity identification, often UUID or structured identifier. 

### `name`
**Purpose**: Human-readable display name for the entity  
**Type**: string or localized object  
**Practice**: MUST/SHOULD  
**Examples**: `"Organic Cotton T-Shirt"`, `{"en-US": "Product Name", "es-ES": "Nombre del Producto"}`  
**Used in**: Product, Category, Campaign, Catalog, Promotion, etc.  
**Notes**: Core display field, supports localization for international use

### `slug`
**Purpose**: URL-friendly identifier for SEO and routing  
**Type**: string  
**Practice**: RECOMMENDED  
**Pattern**: `^[a-z0-9]+(?:-[a-z0-9]+)*$`  
**Examples**: `"organic-cotton-tshirt"`, `"summer-sale-2024"`  
**Used in**: [Product](product/product.md), [Category](product/category.md), etc.  
**Notes**: Used for clean URLs and SEO optimization

---

## Lifecycle and Status Fields

### `status`
**Purpose**: Current lifecycle state of the entity  
**Type**: string (enum)  
**Practice**: SHOULD  
**Standard Values**: `active`, `inactive`, `archived`  
**Entity-Specific Extensions**: 
  - Product: Add `draft` for unpublished items
  - Order: Add `completed`, `cancelled`, `processing`
  - Address: Add `verified`, `invalid` for validation states
**Examples**: [Product](product/product.md): `active`, `inactive`, `archived`, `draft`  
**Used in**: All major entities  
**Notes**: Always include standard three values. Add entity-specific values only when needed

### `type`
**Purpose**: Classification or category of the entity  
**Type**: string (enum)  
**Practice**: SHOULD  
**Examples**: Customer: `person`, `company`; Promotion: `cart`, `product`, `shipping`  
**Used in**: Customer, [Product](product/product.md), Promotion, Catalog, Shipping Method  
**Notes**: Enables polymorphic behavior and specialized handling

---

## Time-related Fields

For a full understanding of timestamp formats and standards, see ISO 8601 specification.

### `created_at`
**Purpose**: Timestamp when the entity was created  
**Type**: ISO 8601 timestamp  
**Practice**: SHOULD  
**Format**: `"2024-01-15T10:30:00Z"`  
**Used in**: All entities  
**Notes**: Essential for audit trails and data governance

### `updated_at`
**Purpose**: Timestamp when the entity was last modified  
**Type**: ISO 8601 timestamp  
**Practice**: SHOULD  
**Format**: `"2024-01-15T14:45:00Z"`  
**Used in**: All entities  
**Notes**: Enables change tracking and cache invalidation

### `valid_from`
**Purpose**: Start of temporal validity period  
**Type**: ISO 8601 timestamp  
**Practice**: RECOMMENDED  
**Examples**: Price validity start, promotion validity, contract validity  
**Used in**: Pricing, Promotion, Contract, License  
**Notes**: Use for temporal validity periods (when something is applicable/valid)

### `valid_to`
**Purpose**: End of temporal validity period  
**Type**: ISO 8601 timestamp  
**Practice**: RECOMMENDED  
**Examples**: Price validity end, promotion expiry, contract end  
**Used in**: Pricing, Promotion, Contract, License  
**Notes**: Paired with `valid_from` for validity periods

### `starts_at` / `ends_at`
**Purpose**: Event or activity scheduling  
**Type**: ISO 8601 timestamp  
**Practice**: RECOMMENDED  
**Examples**: Campaign execution, sale event, maintenance window  
**Used in**: Campaign, Event, Appointment  
**Notes**: Use for scheduled activities/events, not validity periods

### `expires_at`
**Purpose**: Single expiration point (not a range)  
**Type**: ISO 8601 timestamp  
**Practice**: RECOMMENDED  
**Examples**: Cart expiration, session timeout, token expiry  
**Used in**: Cart, Session, Token, Coupon Instance  
**Notes**: Use when something expires and becomes invalid

---

## Localization Fields

For a full understanding, consult the [Language](utilities/language.md) utility object.

### `description`
**Purpose**: Detailed description or explanation of the entity  
**Type**: string or localized object  
**Practice**: SHOULD/RECOMMENDED  
**Examples**: Product descriptions, campaign details, category explanations  
**Used in**: Product, Category, Campaign, Promotion  
**Notes**: Supports rich content and multiple languages

### `locale`
**Purpose**: Language and regional settings combined  
**Type**: string  
**Practice**: SHOULD  
**Format**: BCP 47 locale identifiers (language-region)  
**Examples**: `"en-US"`, `"es-ES"`, `"fr-FR"`, `"pt-BR"`  
**Used in**: Store, Language utility, localized content, user preferences  
**Notes**: Combines language and region for complete localization context. Preferred over separate language/region fields

---

## Integration Fields

### `external_references`
**Purpose**: Cross-system identifiers for integration and orchestration  
**Type**: object (dictionary)  
**Practice**: SHOULD  
**Examples**: `{"pim_id": "PIM-123", "erp_id": "ERP-456", "cms_id": "CMS-789"}`  
**Used in**: All major entities  
**Notes**: Enables system integration and data synchronization

### `source`
**Purpose**: Identifies the originating system or authority  
**Type**: string  
**Practice**: SHOULD (in extensions)  
**Examples**: `"pim"`, `"erp"`, `"cms"`, `"marketing_platform"`  
**Used in**: Extensions across all entities  
**Notes**: Critical for data lineage and conflict resolution

---

## Financial Fields

For a full understanding of money handling, amounts should follow standard decimal practices with currency codes as per ISO 4217.

### `amount`
**Purpose**: Monetary value amount  
**Type**: number  
**Practice**: SHOULD  
**Used with**: `currency` field  
**Examples**: `29.99`, `1500.00`, `-10.00` (for refunds)  
**Used in**: Money utility, Pricing, Payment, Cart totals  
**Notes**: Always paired with currency, supports negative values

### `currency`
**Purpose**: ISO 4217 currency code  
**Type**: string  
**Practice**: SHOULD  
**Format**: 3-letter uppercase code  
**Examples**: `"USD"`, `"EUR"`, `"GBP"`, `"JPY"`  
**Used in**: All financial contexts  
**Notes**: Essential for multi-currency support

---

## Relationship Fields

Any relationship between entities should be prefixed by the domain you are linking to, for exampple `customer_id`, `product_id`, `parent_id` etc.

---

## Content and Media Fields

### `media` / `primary_image`
**Purpose**: Associated media assets and visual content  
**Type**: Media utility object or array  
**Practice**: SHOULD/COULD  
**Examples**: Product images, category banners, campaign visuals  
**Used in**: Product, Category, Campaign, Catalog  
**Notes**: Uses Media utility object for consistency

### `tags`
**Purpose**: Keywords and labels for categorization and search  
**Type**: array of strings  
**Practice**: COULD  
**Examples**: `["organic", "cotton", "sustainable"]`, `["sale", "limited-time"]`  
**Used in**: Product, Media, Category  
**Notes**: Enhances discoverability and organization

---

## Extensibility Fields

### `extensions`
**Purpose**: Namespaced dictionary for domain-specific or optional data  
**Type**: object  
**Practice**: RECOMMENDED  
**Structure**: `{namespace: {data}, source: system}`  
**Examples**: `{"analytics": {"campaign_id": "ABC"}, "seo": {"meta_title": "..."}}`  
**Used in**: All major entities  
**Notes**: Primary mechanism for extensibility without schema modification. You should always include the `source` for traceability.

### `attributes` / `properties`
**Purpose**: Key-value pairs for flexible metadata  
**Type**: object  
**Practice**: COULD  
**Examples**: Product attributes, custom properties  
**Used in**: Product, Cart Items, Categories  
**Notes**: More flexible than structured fields, but less structured than extensions

---

## Data Quality Fields

### `version`
**Purpose**: Version number for optimistic concurrency control  
**Type**: integer  
**Practice**: RECOMMENDED/COULD  
**Examples**: `1`, `5`, `12`  
**Used in**: Inventory, high-frequency update entities  
**Notes**: Prevents lost updates in concurrent scenarios

### `last_used_at` / `last_modified_at`
**Purpose**: Timestamp of last activity or usage  
**Type**: ISO 8601 timestamp  
**Practice**: COULD  
**Examples**: Last coupon usage, last cart update  
**Used in**: Coupon Instance, usage tracking  
**Notes**: Enables activity monitoring and cleanup

---

## Field Naming Conventions

### Reference Fields
- Entity name + `_id` for foreign keys: `customer_id`, `product_id`, `campaign_id`
- `parent_id` for hierarchical relationships
- `external_references` for cross-system mappings (not `reference_ids`)

### Money Fields
- `amount` + `currency` pattern for monetary values
- Descriptive prefixes: `list_price`, `sale_price`, `total_price`
- `_total` suffix for calculated sums: `grand_total`, `tax_total`

### Boolean Fields
- Use `is_` prefix for state/status booleans: `is_active`, `is_verified`, `is_published`
- Positive phrasing: `is_active` instead of `is_inactive`
- Action-oriented capabilities can omit prefix: `trackable`, `stackable`, `backorderable`
- Permission-oriented can omit prefix: `allowed`, `enabled`, `required`

### Collection Fields
- Plural nouns for arrays: `variants`, `categories`, `addresses`
- Descriptive prefixes: `related_products`, `available_methods`

---

## Usage Guidelines

### Required vs Optional
- **MUST**: Critical for entity function (id, core business fields)
- **SHOULD**: Important for standard operations (name, status, timestamps)
- **RECOMMENDED**: Enhances functionality (extensions, references)
- **COULD**: Optional enhancements (tags, attributes, media)

### Localization Support
Fields supporting localization can accept either:
- String: `"Product Name"` (single language)
- Object: `{"en-US": "Product Name", "es-ES": "Nombre del Producto"}` (multi-language)

### Extension Usage
- Use `extensions` for optional, domain-specific data
- Always include `source` field in extension objects
- Namespace by domain: `analytics`, `seo`, `marketing`, `inventory`
- Prefer extensions over core schema modifications
- Structure: `{"namespace": {"field": "value", "source": "system_name"}}`
- Avoid flat extensions or deeply nested structures

---

> This glossary reflects the common patterns and conventions found across the MACH Alliance Common Data Model entities. It serves as a reference for consistent field usage and naming across composable commerce architectures.