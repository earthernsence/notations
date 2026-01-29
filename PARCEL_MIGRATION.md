# Parcel Migration Guide

This document explains the migration from Bili to Parcel for the notations library.

## Configuration Overview

### .parcelrc
- Uses `@parcel/config-default` with the `@parcel/bundler-library` bundler
- The library bundler provides better tree-shaking and proper library output handling

### package.json Targets

The library is built with the following targets:

#### Standard Notations (AD)
- **main**: `dist/ad-notations.umd.js` - UMD format (CommonJS-compatible)
- **module**: `dist/ad-notations.esm.js` - ES Module format
- **ad-min**: `dist/ad-notations.min.js` - Minified UMD format (for CDN/UNPKG)
- **types**: `dist/types/index.d.ts` - TypeScript type definitions

#### Community Notations
- **community**: `dist/community/ad-notations.community.umd.js` - UMD format
- **community-module**: `dist/community/ad-notations.community.esm.js` - ES Module format
- **community-min**: `dist/community/ad-notations.community.min.js` - Minified UMD format
- **types** (community): `dist/community/index.d.ts` - TypeScript type definitions

## Build Scripts

### Building
```bash
# Build everything (AD + Community)
npm run build

# Build only AD notations
npm run build:ad

# Build only Community notations
npm run build:community
```

### How It Works
1. **parcel build** compiles all configured targets
2. **--target** flag can specify which targets to build
3. **copy-minified** scripts copy minified versions to the `docs` folder for easy access

## Output Format Configuration

- **main** target (UMD): Parcel infers CommonJS output from the "main" field
- **module** target (ESM): Parcel infers ES Module output from the "module" field
- **[name]-min** targets: Use `"optimize": true` to force minification for CDN distribution
- **Parcel automatically generates** `.js.map` source maps for all outputs

## Type Declarations

Parcel automatically generates TypeScript type declarations:
- Placed at `dist/types/index.d.ts` for AD notations
- Placed at `dist/community/index.d.ts` for community notations
- Generated from the source TypeScript files with proper export declarations

## Cleanup Notes

The following files from the old Bili configuration can be removed:
- `bili.config.base.ts` - No longer needed
- `bili.config.community.ts` - No longer needed
- Old parcel scripts in package.json have been removed

## Migration Benefits

1. **Simplified Configuration**: Single .parcelrc replaces multiple bili configs
2. **Better Tree-Shaking**: @parcel/bundler-library automatically enables proper scope hoisting
3. **All Formats in One Pass**: Both ESM and UMD built simultaneously for each entry
4. **Automatic Type Generation**: TypeScript declarations generated without manual configuration
5. **Native Library Support**: Parcel is purpose-built for library bundling
