# EBClock — Multi-Timezone Clock Widget

A lightweight HTML page that displays live clocks for one or more time zones. Designed for embedding in an ArcGIS Experience Builder app via the Embed widget, but works in any iframe or browser tab.

Built originally for American Red Cross DR 313-26 (Guam) to give a Director's Brief sidebar live time in Guam, Honolulu, and Eastern. Generalized so the same hosted page can power any DR's clock display just by changing the URL.

## Live URL

```
https://mjmalley-arc.github.io/EBClock/
```

## Quick Start

Paste one of these URLs into the Embed widget's URL field in Experience Builder.

**No parameters — default U.S. time zones** (useful fallback for a template):
```
https://mjmalley-arc.github.io/EBClock/
```
Shows Eastern, Central, Mountain, Pacific.

**Custom zones, auto-derived labels:**
```
https://mjmalley-arc.github.io/EBClock/?zones=Pacific/Guam,Pacific/Honolulu,America/New_York
```
Labels come from the IANA zone name (`Pacific/Guam` becomes "Guam").

**Custom zones and custom labels:**
```
https://mjmalley-arc.github.io/EBClock/?zones=Pacific/Guam,Pacific/Honolulu,America/New_York&labels=Guam,Honolulu,Eastern
```
Labels map 1:1 to zones, in order. Use when the auto-derived label isn't what you want (e.g., you prefer "Eastern" over "New York").

Supports 1 to roughly 5 zones per widget cleanly. Each added zone needs a bit more height on the Embed widget.

## URL Parameters

| Parameter | Required | Description |
|---|---|---|
| `zones` | No | Comma-separated list of IANA time zone names. Defaults to Eastern, Central, Mountain, Pacific. |
| `labels` | No | Comma-separated display labels corresponding 1:1 to `zones`. If omitted, labels are auto-derived from the zone name (part after the `/`, underscores replaced with spaces). |

If a zone name is invalid or misspelled, its time field shows "bad zone" rather than breaking the widget.

## Common Time Zone Reference

| Region | IANA Zone |
|---|---|
| Eastern (ET / EDT / EST) | `America/New_York` |
| Central (CT) | `America/Chicago` |
| Mountain (MT) | `America/Denver` |
| Pacific (PT) | `America/Los_Angeles` |
| Arizona (no DST) | `America/Phoenix` |
| Alaska (AKT) | `America/Anchorage` |
| Hawaii (HST) | `Pacific/Honolulu` |
| Guam / CNMI (ChST) | `Pacific/Guam` |
| Puerto Rico / USVI (AST) | `America/Puerto_Rico` |
| American Samoa (SST) | `Pacific/Pago_Pago` |
| UTC | `UTC` |

Daylight saving transitions are handled automatically — `America/New_York` is EDT in summer and EST in winter without any manual changes.

## Template Workflow (per DR)

1. Duplicate your Experience Builder template for the new DR.
2. On each page containing the clock, click the Embed widget.
3. In its URL field, replace the query string with the zones you want for this DR.
4. Save. End users just see the clocks — they never see or interact with the URL.

Example URLs for past and hypothetical operations:

```
Guam typhoon:
?zones=Pacific/Guam,Pacific/Honolulu,America/New_York&labels=Guam,Honolulu,Eastern

Puerto Rico / USVI:
?zones=America/Puerto_Rico,America/New_York&labels=San%20Juan,Eastern

Alaska:
?zones=America/Anchorage,America/Los_Angeles,America/New_York&labels=Alaska,Pacific,Eastern

Hurricane landfall CONUS with DHQ coordination:
?zones=America/New_York,America/Chicago,America/Los_Angeles
```

## Styling

The page uses a dark navy background (`#002958`) and a bright blue accent bar (`#1f8ae0`) to blend with the Red Cross Experience Builder sidebar theme. Time format is 24-hour with tabular-nums for steady digit width.

To change colors, fonts, or time format, edit the `<style>` block and the `Intl.DateTimeFormat` options in the `<script>` block of `index.html` and commit.

## Recommended Embed Widget Height

| Zones displayed | Height |
|---|---|
| 2 | ~95 px |
| 3 | ~135 px |
| 4 | ~175 px |
| 5 | ~215 px |

Adjust to taste based on your sidebar layout.

## Files

- `index.html` — the clock page (single file, no dependencies, ~1.5 KB)
- `README.md` — this file

## Notes

- Runs entirely in the browser. No server logic, no API calls, no tracking.
- Uses the browser's built-in `Intl.DateTimeFormat` with IANA time zone support — modern browsers handle DST correctly.
- Updates every second via `setInterval`.
- Keeps working if the user's device is offline, since once loaded the page ticks from the local clock.
