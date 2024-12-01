# ColorClaim Protocol

A protocol for registering unique color names on Bitcoin using Ordinals.

## Overview

ColorClaim allows anyone to name unique colors on Bitcoin. Each color can receive only one name, and naming rights are transferred through standard Ordinal transfers.

## Manifesto

Colors are fundamental to human expression and digital creativity. They are a public good that cannot be owned, licensed, or patented. Yet in our digital world, colors lack meaningful identification beyond technical specifications. The ColorClaim Protocol addresses this by enabling permanent color naming on Bitcoin through Ordinals.

We believe:

1. Colors are a universal public good, freely available to all
2. While colors cannot be owned, they deserve meaningful names that enrich human expression
3. The right to name a color should be verifiable and transferable
4. A decentralized registry should preserve color names with absolute immutability and transparency
5. Anyone should be able to participate in color naming
6. The protocol should remain simple
7. The community should guide protocol evolution
8. Names must be able to evolve as language and culture evolve

Our mission is to create an open, global registry of color names on Bitcoin. We commit to:
- Never restricting color usage
- Maintaining global accessibility
- Ensuring complete transparency
- Enabling name transferability
- Supporting cultural evolution through renaming capabilities

## Status

This protocol is Version 1.0.0 and is live on Bitcoin mainnet. While the core protocol is now stable, we welcome community feedback for future improvements.

## Technical Requirements

- Each color claim must be created on a UTXO with a minimum of 600 satoshis
- This ensures the UTXO can support future rename operations
- Prevents issues with Bitcoin's dust limit (546 satoshis)

## Protocol Operations

### 1. Color Claim
The initial claiming of a color name:
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255 128 0",
  "hex": "#FF8000",
  "name": "Digital Orange 🌅"
}
```

### 2. Color Rename
Changing the name of a previously claimed color. Must be inscribed to the same UTXO that contains the original color claim:
```json
{
  "p": "colorclaim",
  "op": "rename",
  "inscription_id": "6037f433df77001a36f3f929a48d0459b03686efab9a91d6ecf90258e4b943f3i0",
  "name": "Vibrant Orange 🔥"
}
```

## Protocol Design

### Rename Operation Design
- Rename inscriptions must be added to the same UTXO that contains the original color claim
- This ensures only the current owner can perform renames
- No additional ownership verification needed
- Automatic enforcement through Bitcoin network rules

### Inscription IDs in Rename Operations
The rename operation uses the inscription ID rather than RGB/HEX values because:
1. Inscription IDs are onchain data derived from transaction IDs and output indices
2. They provide a secure, immutable reference to the original claim
3. Ownership can be verified directly on the Bitcoin blockchain
4. It prevents ambiguity in case of multiple claims for the same color
5. No reliance on indexer state for color ownership verification

### Validation Rules

#### For Claims
- RGB must be three integers (0-255) separated by single spaces
- HEX must be 6 hex digits with # prefix
- RGB and HEX must represent the same color
- Name must be 1-64 characters
- Can contain any Unicode characters including emojis
- Colors are case-insensitive (#FF8000 = #ff8000)
- Names are case-insensitive
- First valid claim wins for each color
- First valid claim wins for each name (compared case-insensitively)

#### For Renames
- Must reference a valid existing claim inscription_id
- Must be inscribed to the same UTXO as the original claim
- New name must follow same character rules (1-64 chars, Unicode/emoji support)
- Names are case-insensitive
- Cannot rename to an existing name (compared case-insensitively)

### Name Comparison Rules
- All name comparisons are done after converting to lowercase
- Whitespace at start and end is trimmed
- Multiple consecutive whitespace characters are collapsed to single space
- Example: "Digital  Orange" = "digital orange" = "  Digital Orange  "

### Valid Name Examples
```json
{"name": "Sunset Orange 🌅"}
{"name": "Deep Blue 🌊"}
{"name": "🌸 Cherry Blossom Pink"}
{"name": "🎨"}
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

## Future Web Standards Integration

The ColorClaim Protocol aims to enhance web standards for color naming and identification through:
- W3C Community Group formation
- Integration with CSS Color Module
- Native browser support
- Developer tooling integration

## Resources

* [Protocol Specification](./SPECIFICATION.md)
* [Community Chat](https://discord.gg/gQbYZbAs)
* [Examples](./examples/)

## Contributing

We welcome feedback and suggestions! Please open an issue or submit a pull request. 

## Community

- GitHub Discussions: Technical proposals
- Discord: Community chat
- Twitter: Updates & announcements

## License

This protocol specification is released under CC0 1.0 Universal.