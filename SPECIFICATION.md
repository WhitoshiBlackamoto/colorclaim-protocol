# ColorClaim Protocol Specification
Version: 1.0.0
Status: Draft

## Abstract
ColorClaim is an open protocol for registering unique color names on Bitcoin through Ordinals inscriptions. Each color can only be claimed once and is transferred through standard Ordinal transfers.

## Protocol Rules

### 1. Inscription Format
All color claims must be inscribed as valid JSON:
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255 128 0",
  "hex": "#FF8000",
  "name": "Digital Orange"
}
```

### 2. Field Specifications
| Field | Type   | Format | Description |
|-------|--------|--------|-------------|
| p     | String | Fixed  | Must be "colorclaim" |
| op    | String | Fixed  | Must be "claim" |
| rgb   | String | `R G B`| Three integers 0-255, space-separated |
| hex   | String | `#RRGGBB` | Standard 6-digit hex color |
| name  | String | Text   | Unique name for the color |

### 3. Validation Rules

#### Color Values
- RGB must be three integers (0-255) separated by single spaces
- HEX must be 6 hex digits with # prefix
- RGB and HEX must represent the same color
- Color values are case-insensitive

#### Names
- 1-64 characters
- Letters, numbers, spaces, basic punctuation
- Must be unique across all claims
- Case-sensitive

### 4. Inscription Rules
- Content-Type: application/json
- First valid claim wins for each color
- First valid claim wins for each name
- Transfers use standard ordinal transfers

## Examples

### Valid
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255 128 0",
  "hex": "#FF8000",
  "name": "Digital Orange"
}
```

### Invalid
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255,128,0",    // Wrong format
  "hex": "#FF8000",
  "name": "Digital Orange"
}
```

## Indexing
Indexers should track:
- Color uniqueness (RGB/HEX)
- Name uniqueness
- Current ownership
- Transfer history