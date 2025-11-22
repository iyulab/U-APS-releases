# U-APS

**Advanced Production Scheduling** - Hybrid Rust/C# scheduling system

## Overview

U-APS is a production scheduling system that combines Rust optimization algorithms with a C# SDK.

```
Input (JSON/Excel) → Scheduling Engine → Output (JSON/Excel)
```

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

## CLI Usage

```bash
# Run scheduler
uaps input.xlsx output.xlsx
uaps input.json output.json

# Update to latest version
uaps update

# Show version
uaps version
```

### Supported Formats

- **Input/Output**: Excel (.xlsx), JSON (.json)
- **Cross-format**: `uaps input.xlsx output.json`

## SDK Usage

```csharp
using UAPS.SDK.Client;
using UAPS.SDK.Interop;
using UAPS.SDK.Models;

// Ensure native library is available (auto-downloads if needed)
await NativeLoader.EnsureLoadedAsync();

// Create jobs and resources
var job = new Job { Id = "J1", Priority = 1 };
job.Operations.Add(new Operation
{
    Id = "O1",
    Time = new OperationTime(0, 60000, 0) // 1 minute processing
});

var resource = new Resource { Id = "M1", Kind = ResourceKind.Equipment };

// Run scheduler
var client = new SchedulerClient();
var request = new ScheduleRequest { Jobs = [job], Resources = [resource] };
var result = client.Schedule(request);

Console.WriteLine($"Makespan: {result.Schedule.MakespanMs}ms");
```

## Input Format

### JSON

```json
{
  "jobs": [
    {
      "id": "J1",
      "priority": 1,
      "dueDate": "2024-12-31T18:00:00",
      "operations": [
        {
          "id": "O1",
          "sequence": 1,
          "setupMs": 5000,
          "processMs": 60000,
          "requiredResources": [
            {
              "resourceType": "Equipment",
              "candidates": ["M1", "M2"]
            }
          ]
        }
      ]
    }
  ],
  "resources": [
    {
      "id": "M1",
      "kind": "Equipment"
    }
  ]
}
```

### Excel

| Sheet | Columns |
|-------|---------|
| Jobs | Id, Priority, DueDate, ProductName |
| Operations | JobId, Id, Sequence, SetupMs, ProcessMs, WaitMs, Candidates |
| Resources | Id, Kind |

## Output Format

The scheduler returns:
- **Assignments**: Operation → Resource → Start/End time
- **Makespan**: Total schedule duration
- **Violations**: Due date violations and constraint breaches

## Manual Download

See [Releases](https://github.com/iyulab/U-APS-releases/releases) for:

### Native Libraries
- `uaps_engine-win-x64.dll` - Windows x64
- `uaps_engine-linux-x64.so` - Linux x64
- `uaps_engine-osx-x64.dylib` - macOS x64
- `uaps_engine-osx-arm64.dylib` - macOS ARM64

### CLI Bundles
- `uaps-cli-win-x64.zip` - Windows x64
- `uaps-cli-linux-x64.tar.gz` - Linux x64
- `uaps-cli-osx-x64.tar.gz` - macOS x64
- `uaps-cli-osx-arm64.tar.gz` - macOS ARM64

## Requirements

- **CLI**: Self-contained (no dependencies)
- **SDK**: .NET 9.0+

## Architecture

```
┌─────────────────────────────────────────┐
│               U-APS                      │
├──────────────────┬──────────────────────┤
│  U-RAS (Rust)    │  APS Engine (Rust)   │
│  - GA, CP-SAT    │  - Manufacturing     │
├──────────────────┼──────────────────────┤
│  UAPS.SDK (C#)   │  UAPS.CLI (C#)       │
│  - Fluent API    │  - Command-line      │
└──────────────────┴──────────────────────┘
```

## License

MIT License

## Related

- [U-RAS](https://github.com/iyulab/U-RAS) - Core optimization algorithms (Rust)
