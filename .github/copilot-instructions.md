# Copilot Instructions for PUF Code Lists

## Repository Context
This repository contains **PUF Code Lists 1.4** - standardized code lists used across the Pagero Universal Format (PUF) suite. These code lists define allowed values for various business document elements, supporting country-specific requirements and international standards compliance.

## Architecture & Purpose

### Code List Structure
- **External References**: Some lists point to external standards (Peppol, ISO, etc.)
- **Mixed Lists**: Combination of PUF-specific and external values
- **PUF-Only Lists**: Values defined exclusively for PUF requirements

### File Organization
```
specification/
├── introduction.adoc              # Overview and version history
├── guidelines.adoc               # Usage guidelines
├── PUF-001-REGISTRATIONDATA.adoc # Registration data codes
├── PUF-002-ADAID.adoc           # ADA ID codes
├── PUF-003-INVOICETYPECODE.adoc  # Invoice type codes
├── PUF-004-CURRENCYCODE.adoc     # Currency codes
├── PUF-005-INVOICEDOBJECTIDENTIFIER.adoc # Object identifiers
├── PUF-006-MIMECODES.adoc        # MIME type codes
├── PUF-007-ENDPOINTSCHEME.adoc   # Endpoint schemes
├── PUF-008-IDENTIFICATIONSCHEME.adoc # ID schemes
├── PUF-009-TAXTYPESCHEME.adoc    # Tax type schemes
├── PUF-010-PAYMENTMEANSCODE.adoc # Payment means
├── PUF-011-ALLOWANCECHARGEREASONCODE.adoc # Allowance/charge reasons
├── PUF-012-TAXCATEGORYCODE.adoc  # Tax categories
├── PUF-013-TAXEXEMPTIONCODE.adoc # Tax exemptions
├── PUF-014-UOMCODE.adoc          # Unit of measure codes
├── PUF-015-ITEMTYPEIDENTIFICATIONCODE.adoc # Item type IDs
├── PUF-016-INVOICESTATUSCODE.adoc # Invoice status codes
├── PUF-017-CLEARANCESTATUSCODE.adoc # Clearance status codes
├── PUF-018-APPLICATIONRESPONSECODE.adoc # Application response codes
├── PUF-019-DOCUMENTREFERENCEDOCTYPECODE.adoc # Document reference types
├── PUF-020-ERRORCLASSIFICATIONCODE.adoc # Error classification codes
├── PUF-021-ROLECODE.adoc         # Role codes
└── support.adoc                  # Support information
```

## Code List Categories

### Country-Specific Identification Schemes (PUF-008)
Contains country-specific identifier schemes beyond standard Peppol/ISO lists:

#### Common Country Patterns
```
[Country Code]:[Scheme Type]
```

**Examples**:
- `CA:GST` - Canada Goods and Services Tax
- `HU:TaxpayerId` - Hungary Tax Number
- `IN:GSTIN` - India GST Identification Number
- `MY:TIN` - Malaysia Tax Identification Number
- `IL:UNION_VAT` - Israel Union VAT Number

#### Basque Country Special Cases
- `TB:PASSPORT` - Passport ID
- `TB:OFFICIAL_ID` - Official ID document
- `TB:RC` - Residence certificate
- `TB:OTHER` - Other identifying document

### Invoice Type Codes (PUF-003)
Defines document types and country-specific subtypes:

#### Standard Types
- `380` - Invoice
- `381` - Credit note
- `383` - Debit note
- `384` - Corrected invoice
- `386` - Prepayment invoice

#### Country-Specific Extensions
**Saudi Arabia**: Additional codes like `388` (Tax Invoice) with required subtypes in `@name` attribute

### External Reference Lists
Several code lists reference external standards:
- **PUF-007 (Endpoint Schemes)**: References Peppol EAS list
- **PUF-004 (Currency Codes)**: References ISO 4217
- **PUF-014 (UOM Codes)**: References UN/ECE Recommendation 20

## Development Workflows

### AsciiDoc Documentation Development

#### Build Command
```bash
asciidoctor -a stylesheet=css/pagero.css -o index.html index.adoc
```

#### Development Workflow
1. **Edit code list files**: Modify specific `PUF-xxx-*.adoc` files
2. **Update version**: Increment version in `index.adoc` and `introduction.adoc`
3. **Recompile**: Run build command to generate `index.html`
4. **Preview**: Open in browser to verify changes
5. **Document changes**: Update changelog in `introduction.adoc`

### Adding New Country-Specific Codes

#### 1. Identify Code List Category
Determine which PUF code list needs extension:
- **Identification schemes** → PUF-008
- **Tax types** → PUF-009
- **Invoice types** → PUF-003
- **Payment means** → PUF-010

#### 2. Follow Country Code Pattern
Use ISO 3166-1 alpha-2 country codes:
```
[CC]:[SCHEME_TYPE]
```

#### 3. Update Appropriate File
Add new section or extend existing country section:
```asciidoc
=== Identification scheme [Country Name]

|===
|Value |Description

|`[CC]:[TYPE]`
|[Description of the identifier type].

|===
```

#### 4. Version Management
- **Minor updates**: Add new codes without breaking existing ones
- **Major updates**: Structural changes requiring version bump
- **Update changelog**: Document all changes in `introduction.adoc`

### Code List Validation Patterns

#### Country Code Validation
- Use ISO 3166-1 alpha-2 format (HR, FR, DE, etc.)
- Consistent with PUF Billing country examples
- Match format used in endpoint schemes and party identification

#### Value Format Consistency
- **Descriptive names**: Clear, unambiguous descriptions
- **Standardized format**: Follow existing patterns in each list
- **No duplicates**: Ensure new codes don't conflict with existing ones

## Common Implementation Patterns

### Using Country-Specific Codes in PUF Documents
```xml
<!-- Party identification with country-specific scheme -->
<cac:PartyIdentification>
    <cbc:ID schemeID="CA:GST">123456789RT0001</cbc:ID>
</cac:PartyIdentification>

<!-- Invoice type with country subtype -->
<cbc:InvoiceTypeCode name="B2B">380</cbc:InvoiceTypeCode>
```

### Endpoint Scheme Usage
```xml
<!-- Croatian OIB scheme -->
<cbc:EndpointID schemeID="9934">12345678901</cbc:EndpointID>

<!-- French SIREN scheme -->
<cbc:EndpointID schemeID="9927">123456789</cbc:EndpointID>
```

### Tax Category Implementation
```xml
<cac:TaxCategory>
    <cbc:ID>S</cbc:ID>  <!-- Standard rate from PUF-012 -->
    <cbc:Percent>25.00</cbc:Percent>
    <cac:TaxScheme>
        <cbc:ID>VAT</cbc:ID>
    </cac:TaxScheme>
</cac:TaxCategory>
```

## Research and Compliance

### Adding New Country Support
1. **Research local requirements**: Study country-specific e-invoicing regulations
2. **Identify unique identifiers**: Find country-specific tax IDs, business numbers, etc.
3. **Check existing standards**: Verify if Peppol/ISO already covers the requirement
4. **Create PUF-specific codes**: Only when no suitable external standard exists
5. **Document rationale**: Explain why new codes are needed

### External Standard Integration
- **Check Peppol updates**: Monitor EAS and ICD list changes
- **ISO compliance**: Ensure currency and country codes remain current
- **Coordinate with PUF Billing**: Align code lists with billing specification examples

## Version History Tracking

### Changelog Format (in introduction.adoc)
```asciidoc
|===
|Version |Date |Description

|1.5 |YYYY-MM-DD |Added [Country] specific codes. Updated [PUF-XXX-LISTNAME].
|===
```

### Release Coordination
- **Synchronize with PUF Billing**: Code list updates should align with billing spec releases
- **Impact assessment**: Consider downstream effects on validation rules and transformations
- **Communication**: Notify stakeholders of breaking changes in major releases

## Best Practices

### When Adding New Codes
- **Follow existing patterns**: Maintain consistency with established formats
- **Country research**: Verify codes match official government standards
- **Clear descriptions**: Write unambiguous explanations for each code
- **Avoid redundancy**: Check if external standards already provide the value

### Documentation Quality
- **Use tables consistently**: Maintain AsciiDoc table format across all lists
- **Link to sources**: Reference official standards and regulations where applicable
- **Version tracking**: Always update the changelog for any modifications

### Integration Considerations
- **Backward compatibility**: New codes should not break existing implementations
- **Validation impact**: Consider how changes affect PUF Billing validation rules
- **Transformation alignment**: Ensure codes work with existing XSLT transformations

This repository serves as the authoritative source for all PUF code values and directly impacts validation and compliance across the entire PUF ecosystem.