# OTA Updater JSON Format

This document describes the JSON format required by the Android Updater app for OTA (Over-The-Air) updates.

## Server Response Format

The updater sends GET requests to your server and expects a JSON response with the following structure:

```json
{
  "response": [
    {
      "datetime": 1230764400,
      "filename": "ota-package.zip",
      "id": "5eb63bbbe01eeed093cb22bb8f5acdc3",
      "romtype": "OneUIX",
      "size": 314572800,
      "url": "https://test.ota-package.zip",
      "version": "15"
    }
  ]
}
```

## Field Descriptions

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `datetime` | integer | Yes | Build date expressed as UNIX timestamp |
| `filename` | string | Yes | Name of the file to be downloaded |
| `id` | string | Yes | Unique identifier for the update |
| `romtype` | string | Yes | Compared with `ro.rom.releasetype` property |
| `size` | integer | Yes | Size of the update in bytes |
| `url` | string | Yes | Direct download URL for the OTA package |
| `version` | string | Yes | Compared with `ro.rom.build.version` property |

## Notes

- The `response` field must be an array, even for single updates
- Additional attributes in the JSON are ignored by the updater
- The `romtype` field is validated against the device's release type property
- The `version` field is used for version comparison to determine if an update is available
- File size should be accurate as it's used for download progress and storage validation

## Example Multiple Updates

```json
{
  "response": [
    {
      "datetime": 1230764400,
      "filename": "test.zip",
      "id": "5eb63bbbe01eeed093cb22bb8f5acdc3",
      "romtype": "OneUI6",
      "size": 314572800,
      "url": "https://test.zip",
      "version": "14"
    },
    {
      "datetime": 1230850800,
      "filename": "test.zip",
      "id": "6fb74ccbf02ffde194dc33cc9g6bdde4",
      "romtype": "OneUI7",
      "size": 315672900,
      "url": "https://test.zip",
      "version": "15"
    }
  ]
}
```
