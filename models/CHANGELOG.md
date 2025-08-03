# MACH Open Data Model Initiative Change Log

All notable changes to the MACH Open Data Model (ODM) will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- New entities and improvements to existing documentation

## [1.0.0] - 2025-10-06
### Added
- **Project Structure and Guides**
    - [Entity Index](entities/README.md) - Complete list of all entities with descriptions
    - [Glossary](entities/glossary.md) - Key terms and definitions used throughout the ODM
    - [Master Entity Template](templates/master-entity-template.md) - Standard format for creating entities
    - [Master Recipe Template](templates/master-recipe-template.md) - Standard format for creating recipes
    - [Creating Entities Guide](templates/creating-canonical-data-models.md) - Step-by-step instructions
    - [Creating ER Diagrams Guide](templates/creating-er-diagrams.md) - How to create entity relationship diagrams
    - [Project README](README.md) - Overview of the MACH ODM initiative
    - [Contributing Guide](CONTRIBUTING.md) - How to contribute to the project

- **Core Entities**
    - **Identity Domain**
        - [Customer](entities/identity/customer.md) - Customer profile and identity management

    - **Product Domain**
        - [Product](entities/product/product.md) - Core product information and attributes
        - [Category](entities/product/category.md) - Product categorization and taxonomy
        - [Product Type](entities/product/product-type.md) - Product type definitions and configurations

    - **Commerce Domain**
        - [Inventory](entities/inventory/inventory.md) - Stock levels and availability
        - [Pricing](entities/pricing/pricing.md) - Price definitions and pricing strategies
        - [Promotion](entities/promotion/promotion.md) - Promotional campaigns and discounts
        - [Coupon Instance](entities/promotion/coupon-instance.md) - Individual coupon usage tracking

    - **Utility Objects**
        - [Address](entities/utilities/address.md) - Physical and digital address structures
        - [Media](entities/utilities/media.md) - Images, videos, and other media assets
        - [Language](entities/utilities/language.md) - Language and localization support

- **Integration Recipes**
    - [PDP Orchestration On-The-Fly](recipes/PDP-orchestration-on-the-fly.md) - Real-time product detail page assembly
    - [PDP Orchestration Optimized Access](recipes/PDP-orchestration-optimized-access.md) - Pre-optimized product page delivery

### Notes
- Initial release establishing the foundation for MACH-compliant data models
- Entities designed to be vendor-neutral and extensible
- All models support both human and machine readability (LLM-friendly)
