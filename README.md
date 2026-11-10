# BKW Energie AG

Tariff data of BKW Energie AG, published by the
[Strompreise Schweiz](https://github.com/Strompreise-Schweiz) initiative. All logic lives in
[`tariff-core`](https://github.com/Strompreise-Schweiz/tariff-core); this repo holds configuration,
corrections and data.

Everything is listed in `https://raw.githubusercontent.com/Strompreise-Schweiz/bkw/main/data/index.json`.

## Dynamic tariffs

These URLs never change. Each file holds today and, once published, tomorrow.

| Tariff | Stable URL |
|---|---|
| BKW Rückliefertarif dynamisch | `https://raw.githubusercontent.com/Strompreise-Schweiz/bkw/main/data/dynamic/feed-in/latest.json` |

The format is the VSE format (HDN 2025, chapter 7.4) with an additional `_meta` block.
Timestamps are Swiss local time with an offset, for example `2026-11-02T00:00:00+01:00`.
Parse the offset and do not assume 96 intervals: a day has 92 in March and 100 in October.
Check `_meta.days[].complete` and `_meta.valid_to` before you use the data.

Archive per day: `data/dynamic/<id>/{raw,vse,normalized}/<year>/<date>.json`.

## Static tariffs (constant, multilevel)

| What | Stable URL |
|---|---|
| Current year | `https://raw.githubusercontent.com/Strompreise-Schweiz/bkw/main/data/tariffs/current.json` |
| A given year | `https://raw.githubusercontent.com/Strompreise-Schweiz/bkw/main/data/tariffs/<year>.json` |

The format is the VSE format (NNMV annex 10, OpenAPI) with additional fields: `tariffs[].id`,
`products` (which tariffs belong together) and `_meta` (source, repairs and corrections).
Prices exclude VAT.

## Structure

| Path | Content | Who changes it |
|---|---|---|
| `config/source.json` | Data sources | Person |
| `curation/<year>.json` | Corrections and product mapping, each with a reason | Person |
| `data/**` | Generated data | GitHub Actions only |

Format details: [`SCHEMA.md`](https://github.com/Strompreise-Schweiz/tariff-core/blob/main/SCHEMA.md).

## License

See [`data/LICENSE`](data/LICENSE) for the data and [`LICENSE`](LICENSE) for the code.
