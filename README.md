# country-flags

A curated collection of 198 SVGs: 197 countries and the reverse of Paraguay's flag. Natural proportions, detailed artwork, no embedded raster images.

The artwork comes from third-party sources; this project collects and adapts it rather than claiming original authorship. [Countries, sources and changes →](CATALOG.md)

## CDN

Use any filename from [`flags/`](flags/):

```html
<img
  src="https://cdn.jsdelivr.net/gh/vdistortion/country-flags@0.1.0/flags/br.svg"
  alt="Brazil"
  width="220"
/>
```

Prefer a published version tag such as `@0.1.0` for stable URLs. You can use the `@main` branch instead, but it changes over time and is therefore not a reliable version reference. CDN updates may also be delayed by caching.

Filenames use lowercase two-letter country codes, plus `xk` for Kosovo and `py-reverse` for Paraguay's reverse. This is a selected collection, not a complete ISO territory list. Afghanistan uses the 2013–2021 tricolour; Syria uses the green–white–black flag with three red stars.

## Country metadata

[`countries.json`](countries.json) contains 197 countries with uppercase codes, English and Russian names, and SVG filenames. To fetch a flag, use its filename under the `flags/` directory; for example, `br.svg` is served at `https://cdn.jsdelivr.net/gh/vdistortion/country-flags@0.1.0/flags/br.svg`. Paraguay has two filenames.

```json
{
  "name": { "en": "Brazil", "ru": "Бразилия" },
  "code": "BR",
  "flag": ["br.svg"]
}
```

The JSON is also available through the CDN:

```text
https://cdn.jsdelivr.net/gh/vdistortion/country-flags@0.1.0/countries.json
```

You can use `@main` instead of a version tag, but the branch can change at any time and is not a reliable version reference. Prepend `flags/` to a filename in the `flag` array when building a CDN URL. Paraguay's array lists both the obverse and reverse SVG filenames.

jsDelivr serves stored files without image resizing. Set SVG display dimensions with HTML/CSS; this does not reduce download size. Smaller raster images would need to be generated and published separately.

## Sources and modifications

182 files were obtained from Wikimedia Commons, 11 from FlagCDN, and 5 from `hampusborgos/country-flags`. The latter sources also distribute artwork originating elsewhere.

SVG dimensions/proportions were matched to a reference collection, colours were adjusted to its pixel values, editor metadata was removed, and SVG markup was optimized. These colours are reference-matched, not a claim of official colour specifications. Exact recorded colour changes and source URLs are in [CATALOG.md](CATALOG.md).

## License

[MIT](LICENSE) applies to this project's original code and documentation, **not to the artwork in `flags/`**. Flags retain their original rights/status. The catalogue records provenance, but individual artwork licences and attribution requirements have not yet been fully audited; a source link alone is not a licence. No blanket licence or claim of ownership is made for the collection's artwork.

## Releases

See [CHANGELOG.md](CHANGELOG.md). No build step or formatter is required. SVG files are published as stored; avoid formatting or optimizing them as part of documentation edits.
