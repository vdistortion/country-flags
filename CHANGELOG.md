# Changelog

## Unreleased

- Added optional English and Russian search aliases for selected countries without changing their display names or existing fields.
- Replaced the `flag` array of filenames in `countries.json` with a `flags` array of objects containing an extensionless `id` and an `aspect_ratio`. SVG URLs use `flags/{id}.svg`. Existing consumers must update when switching to this catalog version.
- Added a stable `region_code` to every country without changing its region assignment or localized labels.

## 0.2.0 — 2026-11-15

- Added `capital_city` with English/Russian names to every country.
- Added `region` with English/Russian names to every country.
- Both additions are backwards-compatible: existing `code`, `name` and `flag` fields are unchanged.

## 0.1.0 — 2026-10-23

- Initial collection: 198 SVG files covering 197 countries and Paraguay's reverse.
- CDN usage example, country catalogue, source links and recorded modifications.
- Country metadata JSON with English/Russian names and SVG paths.
- MIT scope explicitly excludes third-party flag artwork.
