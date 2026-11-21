# country-flags

A curated collection of 198 SVGs: 197 countries and the reverse of Paraguay's flag. Natural proportions, detailed artwork, no embedded raster images.

The artwork comes from third-party sources; this project collects and adapts it rather than claiming original authorship. [Countries, sources and changes →](CATALOG.md)

## CDN: version 0.2.0

To embed a flag, use its filename from [`flags/`](flags/). This example pins the commit for `0.2.0`:

```html
<img
  src="https://cdn.jsdelivr.net/gh/vdistortion/country-flags@3ae2314fe21f7aa68464dcf8fca039d6a935bfb0/flags/br.svg"
  alt="Brazil"
  width="220"
/>
```

Pin a commit SHA or an immutable release tag. Avoid `@main` in production: the branch can change, and jsDelivr may not reflect changes immediately. For `0.2.0`, use the commit SHA because jsDelivr may still serve an older catalog at `@0.2.0`.

The `0.2.0` catalog has an array of SVG filenames in `flag` and a localized object in `capital_city`. It does not have separate locale files. Fetch [`countries.json`](https://cdn.jsdelivr.net/gh/vdistortion/country-flags@3ae2314fe21f7aa68464dcf8fca039d6a935bfb0/countries.json) and append `flags/` and the filename to the same versioned base URL.

Filenames use lowercase two-letter country codes, plus `xk` for Kosovo and `py-reverse` for Paraguay's reverse. This is a selected collection, not a complete ISO territory list. Afghanistan uses the 2013–2021 tricolour; Syria uses the green–white–black flag with three red stars.

## Country metadata: version 0.3.0

[`countries.json`](countries.json) contains language-independent data for 197 countries: country and region codes, flag metadata, and city IDs with capital roles and statuses.

```json
{
  "code": "BR",
  "region_code": "south_america",
  "flags": [
    { "id": "br", "aspect_ratio": [10, 7] }
  ],
  "capitals": [
    { "id": "brasilia" }
  ]
}
```

Each flag `id` is extensionless. Build the SVG URL as `flags/{id}.svg`, relative to `countries.json` at the same version. **`flags[0]` is the primary flag** for display and quizzes; later entries are additional variants. Paraguay lists `py` (obverse) first and `py-reverse` (reverse) second.

### Loading the catalog and a locale

This example requires the published `0.3.0` tag. To test before tagging, replace `0.3.0` in the base URL with the SHA of a pushed commit. Use the same version for the catalog, the locale, and the SVG:

```js
const base = "https://cdn.jsdelivr.net/gh/vdistortion/country-flags@0.3.0/";

async function loadJson(path) {
  const response = await fetch(new URL(path, base));
  if (!response.ok) throw new Error(`${path}: HTTP ${response.status}`);
  return response.json();
}

const [countries, locale] = await Promise.all([
  loadJson("countries.json"),
  loadJson("locales/en.json"),
]);

const country = countries.find(({ code }) => code === "BR");
if (!country) throw new Error("Country BR not found");

const flag = country.flags[0];
const flagUrl = new URL(`flags/${flag.id}.svg`, base).href;
const name = locale.countries[country.code].name;
const region = locale.regions[country.region_code];
const capitals = country.capitals.map(({ id }) => locale.cities[id]);
const [width, height] = flag.aspect_ratio;
```

jsDelivr serves stored files without image resizing. Set SVG display dimensions with HTML/CSS; this does not reduce download size. Smaller raster images would need to be generated and published separately.

### Locales

Load [`locales/en.json`](locales/en.json) or [`locales/ru.json`](locales/ru.json) from the same catalog version. Each file provides country names and aliases by country code, region names by region code, and city names by city ID:

```json
{
  "countries": { "BR": { "name": "Бразилия" } },
  "regions": { "south_america": "Южная Америка" },
  "cities": { "brasilia": "Бразилиа" }
}
```

Use IDs as lookup keys, not as display labels. Keep existing IDs when changing a translation or spelling. Consumers that support a partial locale can fall back to the English dictionary for missing names. Labels for capital roles and statuses are up to the consumer.

### Capitals

`capitals` is always an array. Each entry has a city `id` from the `cities` dictionary and may have a `role` (`administrative`, `legislative`, `judicial`) or a `status` (`de_facto`, `proclaimed`, `claimed`). Status describes how the city is treated as a capital, independently of the functions it serves. The array is not ranked.

`seat_of_government` is an optional city ID for a separately listed seat of government or administrative center. For example, the Netherlands lists Amsterdam as its capital and The Hague as its seat of government. Naoero has an empty `capitals` array and lists Yaren District separately. Palestine lists East Jerusalem as its claimed capital and Ramallah as the administrative center of the Palestinian Authority. Yemen lists Sana'a as its capital and [Aden as the interim seat of its internationally recognized government](https://transcripts.un.org/en/sc/10090).

For display and search, use the names of all listed cities, keeping the government seat distinct from the capital. For a quiz that expects one unqualified answer, a conservative starting point is to use only countries with one capital, no explicit role or status, and no separate government seat.

References for special cases include the [South African government](https://www.gov.za/about-sa/south-africa-glance-0), [Britannica on Eswatini](https://www.britannica.com/place/Lobamba) and [Ramallah](https://www.britannica.com/place/Ramallah), the [Swiss Federal Department of Foreign Affairs](https://www.aboutswitzerland.eda.admin.ch/en/political-system), and UN material on [Naoero](https://micronesia.un.org/en/about/nauru) and [Jerusalem](https://www.un.org/unispal/document/auto-insert-203463).

### Region keys

Use `region_code` for filtering and grouping, and look up the display name in the locale's `regions` dictionary. Region keys are catalog identifiers, not ISO or UN M49 codes. The catalog assigns each country to one of six regions; these assignments reflect the catalog's own grouping.

| `region_code` | English | Russian |
| --- | --- | --- |
| `africa` | Africa | Африка |
| `asia` | Asia | Азия |
| `europe` | Europe | Европа |
| `north_america` | North America | Северная Америка |
| `oceania` | Oceania | Океания |
| `south_america` | South America | Южная Америка |

### Search aliases

Some country entries in a locale have an optional `aliases` array with alternative names or common abbreviations for search. For example, Türkiye (`TR`) has `"aliases": ["Turkey"]` in the English locale.

Use `name` for display. Aliases are not exhaustive or unique identifiers; use `code` to identify countries. For bilingual search, match against both languages. Normalize case, accents and punctuation in the search itself rather than storing spelling variants that only differ in formatting.

### Flag proportions

Each `aspect_ratio` pair is `[width, height]` reduced to the smallest positive integers. It describes the published SVG's declared dimensions, not a fixed display size or an official flag specification. For example, Nepal's `[50, 61]` can be used as CSS `aspect-ratio: 50 / 61`.

After adding a flag or changing its SVG's declared dimensions, update `aspect_ratio` from the SVG's `width` and `height`, reducing both numbers by their greatest common divisor.

### JSON schemas

Use [`countries.schema.json`](countries.schema.json) for `countries.json` and [`locales/locale.schema.json`](locales/locale.schema.json) for each locale. Both use JSON Schema Draft 2020-12 and can be associated with the data files in an editor or used by a validator.

The schemas validate individual files, not cross-file references, uniqueness of country codes and flag IDs, or whether aspect ratios match the SVG dimensions.

## Sources and modifications

182 files were obtained from Wikimedia Commons, 11 from FlagCDN, and 5 from `hampusborgos/country-flags`. The latter sources also distribute artwork originating elsewhere.

SVG dimensions/proportions were matched to a reference collection, colours were adjusted to its pixel values, editor metadata was removed, and SVG markup was optimized. These colours are reference-matched, not a claim of official colour specifications. Exact recorded colour changes and source URLs are in [CATALOG.md](CATALOG.md).

## License

[MIT](LICENSE) applies to this project's original code and documentation, **not to the artwork in `flags/`**. Flags retain their original rights/status. The catalogue records provenance, but individual artwork licences and attribution requirements have not yet been fully audited; a source link alone is not a licence. No blanket licence or claim of ownership is made for the collection's artwork.

## Releases

See [CHANGELOG.md](CHANGELOG.md). No build step or formatter is required. SVG files are published as stored; avoid formatting or optimizing them as part of documentation edits.
