# ColorClaim Protocol Examples

This directory contains examples of valid and invalid color claims to help implementers understand the protocol requirements.

## Valid Claims
Examples of correctly formatted color claims that would be accepted by the protocol.

### Basic Colors (`valid-claims/basic-colors.json`)
Simple examples using primary colors to demonstrate the basic format.

### Mixed Colors (`valid-claims/mixed-colors.json`)
More complex examples showing various color combinations and naming patterns.

## Invalid Claims
Examples of invalid claims that would be rejected by the protocol, along with explanations of why they're invalid.

### Format Errors (`invalid-claims/format-errors.json`)
Examples of common formatting mistakes:
- Incorrect RGB value separators
- Missing values
- Out of range values

### Mismatch Errors (`invalid-claims/mismatch-errors.json`)
Examples of validation errors:
- RGB/HEX mismatches
- Missing HEX prefix
- Invalid names

## Using These Examples
You can use these examples to:
1. Test your implementation
2. Understand the protocol requirements
3. Verify your validation logic

Each example includes comments explaining why certain claims are valid or invalid.