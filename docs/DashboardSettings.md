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
    { "Name": "CoolDash1", "Executable": "C:\\Dashboards\\CoolDash1.exe" },
    { "Name": "ExtraDash", "Executable": "C:\\Dashboards\\ExtraDash.exe" }
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

By default, the DS looks for dashboards (such as Elastic) installed in the WPILib folder for the current year (e.g. `C:\Users\Public\wpilib\2027`). Each WPILib release installs dashboards into its own per-year folder. You can find the current dashboard year right above the dashboard selector on the settings page.

`DashboardYearOverrides` lets you redirect the DS to look in a different year's WPILib folder for a specific year's dashboards. This is useful during pre-release periods — for example, when a new alpha or beta WPILib version ships dashboards under a new folder name, you can redirect the DS to find them there without waiting for an official DS update. It also prevents the override from accidentally applying to future DS versions that already point to the correct, updated folder.

Each entry has the following fields:

| Field                     | Type   | Description                                                                                        |
|---------------------------|--------|----------------------------------------------------------------------------------------------------|
| `DashboardYearToOverride` | string | The WPILib year string that the DS would normally use when looking for dashboards.                 |
| `DashboardOverrideYear`   | string | The WPILib year string to use instead, pointing the DS to that year's WPILib installation folder.  |
