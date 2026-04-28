# Dashboard Settings

The FIRST Driver Station supports configuring custom dashboards and overriding the default WPILib year used for dashboard lookups. This is done via a JSON configuration file named `DriverStationDashboardSettings.json`.

## File Location

Place the file in the DS configuration directory:

- **Windows**: `C:\Users\Public\Documents\FIRSTDriverStation\DriverStationDashboardSettings.json`
- **Unix (Linux/macOS)**: `~/.firstds/DriverStationDashboardSettings.json`

## Configuration

The file is a JSON object with two optional top-level keys: `CustomDashboards` and `DashboardYearOverrides`.

```json
{
  "CustomDashboards": [
    { "Name": "CoolDash1", "Executable": "C:\\Dashboards\\CoolDash1App" },
    { "Name": "ExtraDash", "Executable": "C:\\Dashboards\\ExtraDashApp" }
  ],
  "DashboardYearOverrides": [
    { "DashboardYearToOverride": "2027_alpha5", "DashboardOverrideYear": "2027_alpha6" }
  ]
}
```

### CustomDashboards

`CustomDashboards` is an array of custom dashboards to add to the dashboard selector in the DS. Each entry has the following fields:

| Field        | Type   | Description                                             |
|--------------|--------|---------------------------------------------------------|
| `Name`       | string | The display name shown in the DS dashboard selector.    |
| `Executable` | string | The full path to the dashboard executable to launch.    |

### DashboardYearOverrides

`DashboardYearOverrides` is an array of WPILib year overrides that redirect the DS when looking up default dashboards for a given year. Each entry has the following fields:

| Field                    | Type   | Description                                                   |
|--------------------------|--------|---------------------------------------------------------------|
| `DashboardYearToOverride`| string | The WPILib year string whose dashboard location is overridden. |
| `DashboardOverrideYear`  | string | The WPILib year string to use instead when locating dashboards.|
