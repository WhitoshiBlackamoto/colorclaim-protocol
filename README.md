# ColorClaim Protocol

A protocol for registering unique color names on Bitcoin using Ordinals.

## Overview
ColorClaim allows anyone to claim and name unique colors on Bitcoin. Each color can only be claimed once, and ownership is transferred through standard Ordinal transfers.

## Status
This protocol is currently in DRAFT status and open for community feedback.

## Quick Start
A valid color claim looks like this:
```json
{
  "p": "colorclaim",
  "op": "claim",
  "rgb": "255 128 0",
  "hex": "#FF8000",
  "name": "Digital Orange"
}
```

## Resources
- [Protocol Specification](SPECIFICATION.md)
- [Community Chat](link-to-discord-or-telegram)
- [Examples](examples/)

## Contributing
We welcome feedback and suggestions! Please open an issue or submit a pull request.

## License
This protocol specification is released under CC0 1.0 Universal.