# .NET 8 Migration Guide

This document outlines the changes made to migrate the Raspberry Debugger project to support .NET 8 and ensure compatibility with Visual Studio 2022 and Visual Studio 2026.

## Summary of Changes

### 1. Main Project Updates (RaspberryDebugger.csproj)

- **Target Framework**: Updated from .NET Framework 4.7.2 to .NET Framework 4.8 (required for latest VS SDK)
- **Minimum Visual Studio Version**: Updated from 16.0 to 17.0 (Visual Studio 2022)
- **NuGet Package Updates**:
  - Microsoft.VisualStudio.SDK: Updated to version 17.8.37221
  - Microsoft.VisualStudio.ManagedInterfaces: Updated to version 17.6.268
  - Microsoft.VisualStudio.ProjectSystem: Updated to version 17.6.268
  - Microsoft.VisualStudio.AppDesigner: Updated to version 17.0.31903
  - Neon packages: Updated to version 3.10.0
  - Polly: Updated to version 8.4.2
  - StreamJsonRpc: Updated to version 2.18.33

### 2. VSIX Manifest Updates (source.extension.vsixmanifest)

- **Version**: Bumped from 3.3 to 3.4
- **Visual Studio Support**: Extended to support VS 2022 and VS 2026 (version range [17.0, 19.0))
- **Description**: Updated to mention .NET 8 support and VS 2026 compatibility
- **Tags**: Added "2026" and "dotnet net8" tags

### 3. .NET 8 SDK Support

- **SDK Catalog**: Added .NET 8.0 SDK entries for ARM32 and ARM64 architectures
- **Version Support**: Updated supported versions text to include .NET 8
- **Error Messages**: Updated to reflect support for .NET 3.1, 5, 6, 7, and 8

### 4. Test Project Updates

Updated all test projects to target .NET 8:
- **Blinkie/Blinkie.csproj**: net6.0 → net8.0, updated System.Device.Gpio to 3.2.0
- **Blinkie/WebApp/WebApp.csproj**: net6.0 → net8.0
- **Blinkie/TestLib/TestLib.csproj**: netstandard2.1 → net8.0
- **Blinkie31/Blinkie31.csproj**: Kept as netcoreapp3.1 for backward compatibility testing

### 5. Solution File Updates

- **Visual Studio Version**: Updated to 17.12.35506.00
- **Minimum Version**: Updated to 17.0.31903.59

### 6. Documentation Updates

- **README.md**: Updated to reflect .NET 8 support and VS 2022/2026 compatibility
- **Version References**: Updated all version references from "3.1 + 6.0 and STS 7.0" to "3.1, 5, 6, 7, and 8"

## Breaking Changes

### For Users
- **Visual Studio Requirements**: Now requires Visual Studio 2022 or later (minimum 17.0)
- **Framework Requirements**: The extension now requires .NET Framework 4.8

### For Developers
- **Build Environment**: Requires Visual Studio 2022 SDK or later for building the extension
- **Test Projects**: Most test projects now target .NET 8 (except Blinkie31 which remains on .NET Core 3.1)

## Migration Steps for Existing Projects

1. **Update Visual Studio**: Ensure you're using Visual Studio 2022 (17.0) or later
2. **Update .NET Runtime**: Install .NET 8 runtime on your development machine
3. **Update Target Projects**: Consider updating your Raspberry Pi projects to target .NET 8 for latest features and performance improvements
4. **Raspberry Pi Setup**: The debugger will automatically install .NET 8 runtime on the Raspberry Pi when needed

## New Features

- **Extended .NET Support**: Full support for .NET 8 applications
- **Visual Studio 2026 Ready**: Prepared for future Visual Studio versions
- **Improved Performance**: Benefits from .NET 8 performance improvements
- **Latest Dependencies**: Updated to use latest stable versions of all dependencies

## Compatibility Matrix

| .NET Version | Support Status | Notes |
|--------------|----------------|-------|
| .NET Core 3.1 | ✅ Supported | LTS, widely used |
| .NET 5 | ✅ Supported | Out of support, upgrade recommended |
| .NET 6 | ✅ Supported | LTS, recommended |
| .NET 7 | ✅ Supported | STS, out of support |
| .NET 8 | ✅ Supported | LTS, latest and recommended |

| Visual Studio Version | Support Status |
|----------------------|----------------|
| VS 2019 | ❌ No longer supported |
| VS 2022 | ✅ Fully supported |
| VS 2026 | ✅ Ready for future support |

## Testing Recommendations

1. Test with existing .NET Core 3.1 projects to ensure backward compatibility
2. Test with new .NET 8 projects to verify new functionality
3. Verify debugging works correctly on both ARM32 and ARM64 Raspberry Pi devices
4. Test web application debugging with ASP.NET Core 8.0

## Known Issues

- Network connectivity issues may occur when downloading Visual Studio SDK packages during build
- Some older NuGet package versions may have compatibility issues with .NET 8

## Future Considerations

- Monitor for .NET 9 release and prepare for future updates
- Consider dropping support for older .NET versions (5, 7) in future releases
- Evaluate Visual Studio 2026 compatibility when it becomes available