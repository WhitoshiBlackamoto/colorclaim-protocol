# ColorClaim Protocol Specification
Version: 1.0.0

## Abstract
ColorClaim is an open protocol for registering unique color names on Bitcoin through Ordinals inscriptions. Each color can only be claimed once and is transferred through standard Ordinal transfers. The protocol supports both initial color claims and subsequent name changes through a rename operation.

## Technical Requirements
- Each color claim must be created on a UTXO with a minimum recommended of 600 satoshis
- This requirement ensures UTXOs can support future rename operations
- Prevents issues with Bitcoin's dust limit (546 satoshis)

## Protocol Rules

### 1. Inscription Formats

#### Color Claim
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255 128 0",
  "hex": "#FF8000",
  "name": "Digital Orange 🌅"
}
```

#### Color Rename
```json
{
  "p": "colorclaim",
  "op": "rename",
  "inscription_id": "6037f433df77001a36f3f929a48d0459b03686efab9a91d6ecf90258e4b943f3i0",
  "name": "Vibrant Orange 🔥"
}
```

### 2. Field Specifications
| Field | Type | Format | Description |
|-------|------|--------|-------------|
| p | String | Fixed | Must be "colorclaim" |
| op | String | Fixed | Must be "claim" or "rename" |
| rgb | String | `R G B` | Three integers 0-255, space-separated (claim only) |
| hex | String | `#RRGGBB` | Standard 6-digit hex color (claim only) |
| name | String | Text | Unique name for the color (1-64 chars, Unicode support) |
| inscription_id | String | Fixed | Original claim inscription ID (rename only) |

### 3. Validation Rules

#### Color Values (Claim Operation)
- RGB must be three integers (0-255) separated by single spaces
- HEX must be 6 hex digits with # prefix
- RGB and HEX must represent the same color
- Color values are case-insensitive (#FF8000 = #ff8000)
- First valid claim wins for each color

#### Names (Both Operations "claim" and "rename")
- 1-64 characters in length
- Can contain any Unicode characters including emojis
- Names are case-insensitive
- Whitespace at start and end is trimmed
- Multiple consecutive whitespace characters are collapsed to single space
- Must be unique across all claims (compared case-insensitively)
- Examples: "Deep Blue 🌊" = "deep blue 🌊"

#### Rename Operation
- Must reference a valid existing claim inscription_id
- Must be inscribed to the same UTXO as the original claim
- Cannot rename to an existing name (compared case-insensitively)
- Only the current owner of the original inscription can rename

### 4. Inscription Rules
- Content-Type: application/json
- Claims must be on UTXOs with minimum 600 satoshis
- Rename operations must be on the same UTXO as original claim
- Transfers use standard ordinal transfers

## Examples

### Valid Claims
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255 128 0",
  "hex": "#FF8000",
  "name": "Digital Orange"
}
```
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "0 128 255",
  "hex": "#0080FF",
  "name": "Ocean Blue 🌊"
}
```

### Valid Rename
```json
{
  "p": "colorclaim",
  "op": "rename",
  "inscription_id": "6037f433df77001a36f3f929a48d0459b03686efab9a91d6ecf90258e4b943f3i0",
  "name": "Vibrant Orange 🔥"
}
```

### Invalid Operations
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255,128,0",    // Wrong RGB format
  "hex": "#FF8000",
  "name": "Digital Orange"
}
```
```json
{
  "p": "colorclaim",
  "op": "rename",
  "name": "New Name"     // Missing inscription_id
}
```

## Indexing Requirements

Indexers must track:
1. Original claims
   - Color uniqueness (RGB/HEX, case-insensitive)
   - Original inscription IDs
   - Initial names (stored with original casing but compared case-insensitively)
2. Current state
   - Current ownership
   - Current names (stored with original casing but compared case-insensitively)
   - UTXO containing the inscription
3. Historical data
   - Name history (preserving original casing)
   - Ownership transfers

## Future Compatibility
The protocol may evolve to support additional features in future versions while maintaining backwards compatibility with Version 1.0.0.