# Changelog

## 0.3.0 — 2026-11-21

### Breaking changes

- `countries.json` now contains language-independent data only. Load country names and aliases by country code, region labels by `region_code`, and city names by ID from `locales/en.json` or `locales/ru.json`. Use the same version for the catalog, locale, and SVGs.
- `flag` (an array of filenames) became `flags` (an ordered array of objects with an extensionless `id` and an `aspect_ratio`). Use `flags[0]` as the primary flag and build SVG URLs as `flags/{id}.svg`.
- `capital_city` (one localized name) became `capitals` (an array of city IDs with optional roles and statuses). Use the optional `seat_of_government` ID for an administrative center listed separately.

### Other changes

- Added optional English and Russian search aliases for selected countries.
- Added a stable `region_code` to every country without changing its region assignment or localized labels.
- Updated the display names for [Czechia](https://mzv.gov.cz/jnp/en/foreign_relations/public_diplomacy/digital_diplomacy/using_Czechia.html), [Naoero](https://www.un.org/en/about-us/member-states/naoero), and [Timor-Leste in Russian](https://gadebate.un.org/ru/80/timor-leste). Previous display labels remain searchable as aliases.
- Added JSON schemas for the country catalog and locale files.

## 0.2.0 — 2026-11-15

- Added `capital_city` with English/Russian names to every country.
- Added `region` with English/Russian names to every country.
- Both additions are backwards-compatible: existing `code`, `name` and `flag` fields are unchanged.

## 0.1.0 — 2026-10-23

- Initial collection: 198 SVG files covering 197 countries and Paraguay's reverse.
- CDN usage example, country catalogue, source links and recorded modifications.
- Country metadata JSON with English/Russian names and SVG paths.
- MIT scope explicitly excludes third-party flag artwork.
