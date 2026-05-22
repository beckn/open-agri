# Agri Services Extension Schema Pack

**Protocol Version:** Beckn 2.0
**Schema Version:** 2.1.0
**Semantic Model:** generalised
**Status:** Initial release
**Base Pack:** generic-service v2.1.0

## Overview

The Agri Services Extension schema pack extends the generic-service schemas with agriculture-specific fields and concepts. It provides specialized schemas for representing livestock and farm-related services in the Beckn protocol.

## Schemas

This pack includes 3 extension schemas that extend the generic-service pack:

1. **AgriServiceResource** - Extends ServiceResource with livestock types, service categories (vaccination, advisory, etc), provider types (vet, agrovet, extension officer), and hierarchical geographic coverage (village/ward/county)

2. **AgriServiceOffer** - Extends ServiceOffer with input brand or variety specification for agricultural input services

3. **AgriServicePerformance** - Extends ServicePerformance with farm-specific location details and livestock count/type tracking

## Use Cases

This schema pack is designed for:

- Veterinary services (vaccination, health screening, advisory)
- Poultry vaccination and management services
- Agricultural extension officer services
- Agricultural input supply (seeds, feed, fertilizer)
- Farmer training and advisory services
- Livestock disease screening and monitoring

## Architecture

These schemas extend the generic-service pack via OpenAPI `allOf` references:

```yaml
AgriServiceResource:
  allOf:
    - $ref: "../../services/ServiceResource/v2.1/attributes.yaml#/components/schemas/ServiceResource"
```

This allows AgriServiceResource to inherit all ServiceResource fields while adding agriculture-specific properties.

## Transaction Flow

1. **Discovery** (on_discover): AgriServiceResource and AgriServiceOffer in catalogs (includes inherited generic-service fields)
2. **Confirmation** (on_confirm): AgriServicePerformance in contract performance (includes inherited generic-service fields)

## Example Scenario

A veterinarian operating in a highland farming region offers poultry vaccination services (illustrative):

- **Resource**: Poultry vaccination service (serviceCategory: VACCINATION, providerType: VET, livestockTypes: [POULTRY], coverage: village/ward/county level)
- **Offer**: Service booking terms (quantity, advance booking, cancellation policies)
- **Performance**: Vaccination visit to a smallholder farmer's 200-bird poultry farm at the recorded farm location (GeoJSON Point), with follow-up scheduling

## Getting Started

Each schema directory contains:
- `attributes.yaml` - OpenAPI 3.1.1 specification with allOf extension
- `context.jsonld` - JSON-LD context for new and inherited properties
- `vocab.jsonld` - Vocabulary definitions for new enums
- `profile.json` - Schema metadata and configuration
- `renderer.json` - UI rendering templates
- `README.md` - Schema-specific documentation
- `examples/` - Example JSON payloads

## Testing

Run the included test suite to validate the schemas:

```bash
cd tests
python run_tests.py
```

The test suite validates:
- OpenAPI 3.1.1 compliance with allOf resolution
- JSON-LD correctness with inherited fields
- Property coverage (both new and inherited)
- Example payload structure

## Namespace Prefixes

- `asr:` - AgriServiceResource namespace
- `asof:` - AgriServiceOffer namespace
- `aspm:` - AgriServicePerformance namespace

Inherited prefixes (from generic-service):
- `sr:` - ServiceResource (for inherited fields)
- `sof:` - ServiceOffer (for inherited fields)
- `spm:` - ServicePerformance (for inherited fields)

## Next Steps

- Review individual schema READMEs for detailed specifications
- Examine examples for typical payload structures
- Run tests to ensure compliance
- Consider further vertical extensions if needed
