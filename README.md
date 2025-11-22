# U-APS Releases

Binary releases for [U-APS](https://github.com/iyulab/U-APS) (Advanced Production Scheduling).

## Installation

### CLI

```bash
# Windows (PowerShell)
irm https://raw.githubusercontent.com/iyulab/U-APS-releases/main/install.ps1 | iex

# Linux/macOS
curl -fsSL https://raw.githubusercontent.com/iyulab/U-APS-releases/main/install.sh | bash
```

### SDK (NuGet)

```bash
dotnet add package UAPS.SDK
```

## Update

```bash
uaps update
```

## Manual Download

See [Releases](https://github.com/iyulab/U-APS-releases/releases) for:
- Native libraries (Windows, Linux, macOS)
- CLI bundles (self-contained)
- NuGet packages

## License

MIT License
