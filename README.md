# discover-gh-r8

Rhino 8 compatible fork of [colidescope/discover-gh](https://github.com/colidescope/discover-gh).

Discover is a genetic algorithm optimization plugin for Grasshopper/Rhino,
developed by [Colidescope](https://colidescope.com). This fork ports the 
Grasshopper plugin component (.gha) to run on Rhino 8 without modification 
to the Discover server or web UI.

## What Changed

| Change | Reason |
|--------|--------|
| Retargeted from `net45` to `net48` | Rhino 8 dropped .NET Framework 4.5 support |
| Replaced `JavaScriptSerializer` with `System.Text.Json 9.0.0` | Removes `System.Web.Extensions` dependency missing in Rhino 8 |
| Fixed `PingServer` timeout: 1ms → 5000ms | 1ms caused silent timeout in R8, stalling optimization after generation 1 |
| Replaced deprecated `GetViewList(bool, bool)` with `GetViewList(RhinoViewListFilter.StandardViews)` | API deprecated in Rhino 8 SDK |
| Extracted `SERVER_BASE` constant to `Helpers.cs` | Single-point port configuration for non-default setups |
| Migrated to `PackageReference` format | Enables `dotnet restore` without requiring `nuget.exe` |
| Updated Rhino/Grasshopper NuGet references from v6 to v8.29 | Correct SDK for Rhino 8 |

## Requirements

- Rhino 7 or Rhino 8 (Windows)
- [Discover server v19.12](https://colidescope.github.io/discover/) — unchanged, no modifications needed
- Python 3.9.x for the Discover server
- .NET Framework 4.8 (included with Windows 10/11)

## Building
dotnet restore Discover.sln
msbuild Discover/Discover.csproj -p:Configuration=Release

Output: `Discover/bin/Discover.gha`

## Installation

1. Build the `.gha` or download from [Releases](../../releases)
2. Copy `Discover.gha` to your Grasshopper components folder:
   `%AppData%\Roaming\Grasshopper\Libraries`
3. Restart Rhino
4. Launch the Discover server via `discover.bat`
5. Open Grasshopper — the Discover tab should appear

## Changing the Server Port

If port 5000 is unavailable (macOS reserves it for AirPlay Receiver):

1. In `Helpers.cs`, change:
   `public const string SERVER_BASE = "http://127.0.0.1:5000";`
   to your desired port e.g. `5001`
2. In the Discover server `server.py`, change the matching port
3. Rebuild

## Credits

Original plugin by [Colidescope](https://colidescope.com).  
Rhino 8 port by [Ihor Koshkin](https://github.com/ikoshkin)
