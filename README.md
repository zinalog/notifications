# ZinaLog Global Notifications

This repository provides global notifications for self-hosted ZinaLog installations.

ZinaLog periodically fetches `notifications.json` to display announcements such as maintenance notices, security advisories, upgrade recommendations, and other important project updates.

> **Never include secrets, credentials, personal data, or installation-specific information. Everything in this repository should be treated as public.**

## Files

```text
.github/workflows/validate.yml
notifications.json
schema.json
README.md
```

- `notifications.json` — published notifications consumed by ZinaLog.
- `schema.json` — defines the required notification structure.
- `validate.yml` — validates notification data against the schema.

## Notification format

```json
{
  "version": 1,
  "notifications": [
    {
      "id": "2026-09-27-maintenance",
      "type": "warning",
      "title": "Scheduled Maintenance",
      "message": "ZinaLog services will undergo scheduled maintenance.",
      "publishedAt": "2026-09-27T10:00:00Z",
      "expiresAt": "2026-09-30T10:00:00Z",
      "minVersion": null,
      "maxVersion": null,
      "dismissible": true,
      "url": null
    }
  ]
}
```

## Fields

| Field | Required | Description |
|---|---|---|
| `id` | Yes | Unique notification identifier. Never reuse an ID. |
| `type` | Yes | `info`, `warning`, `error`, or `success`. |
| `title` | Yes | Short notification title. |
| `message` | Yes | Plain-text notification message. |
| `publishedAt` | Yes | ISO 8601 UTC publication timestamp. |
| `expiresAt` | No | Expiration timestamp or `null`. |
| `minVersion` | No | Minimum applicable ZinaLog version or `null`. |
| `maxVersion` | No | Maximum applicable ZinaLog version or `null`. |
| `dismissible` | Yes | Whether the notification can be dismissed. |
| `url` | No | HTTPS URL for more information or `null`. |

## Version targeting

`minVersion` and `maxVersion` control which ZinaLog versions receive a notification.

- Both `null` → all versions.
- `minVersion` only → that version and newer.
- `maxVersion` only → that version and older.
- Both set → versions within the inclusive range.

## Publishing a notification

Add a new object to the `notifications` array.

Use a unique ID, preferably:

```text
YYYY-MM-DD-short-description
```

For temporary notifications, set `expiresAt` so installations automatically stop displaying them.

Notification IDs must **never be reused**, even after a notification expires or is removed.

## Rules

- Keep titles and messages concise.
- Messages must contain plain text only. No HTML or JavaScript.
- Use ISO 8601 UTC timestamps.
- URLs must use HTTPS.
- Never publish secrets or installation-specific information.
- Prefer expiration dates for temporary notifications.
- Breaking changes to the notification format require incrementing the top-level `version`.
- Changes to `notifications.json` must conform to `schema.json`.

ZinaLog should also validate remote notification data at runtime before displaying it. Invalid or unavailable notification data must not affect normal ZinaLog operation.