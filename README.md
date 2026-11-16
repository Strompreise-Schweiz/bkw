# BKW Energie AG

[![VSE 1.0.0](https://img.shields.io/endpoint?url=https%3A%2F%2Fdata.strompreise-schweiz.ch%2Fbkw%2Fbadges%2Fvse-1.0.0.json)](data/checks.json)
[![VSE 1.0.1](https://img.shields.io/endpoint?url=https%3A%2F%2Fdata.strompreise-schweiz.ch%2Fbkw%2Fbadges%2Fvse-1.0.1.json)](data/checks.json)

The badges show whether the yearly tariff file the utility publishes follows the
[OpenAPI definitions of the VSE](https://github.com/Strompreise-Schweiz/tariff-core/tree/main/schemas/vse). Details in [`data/checks.json`](data/checks.json).

Tariff data of BKW Energie AG, published by the
[Strompreise Schweiz](https://github.com/Strompreise-Schweiz) initiative. All logic lives in
[`tariff-core`](https://github.com/Strompreise-Schweiz/tariff-core); this repo holds configuration,
corrections and data.

Everything is listed in `https://data.strompreise-schweiz.ch/bkw/index.json`.

## Where what is

| Folder | Holds | Changed by |
|---|---|---|
| [`config/source.json`](config/source.json) | where the data comes from | a person |
| `curation/<year>.json` | corrections and products, each with a reason | a person |
| `data/sources/` | the files of BKW, unchanged: the yearly file and in `days/` what the APIs sent | GitHub Actions |
| `data/tariffs/` | all tariffs, repaired to VSE 1.0.1 and corrected, with the day prices of the dynamic tariffs | GitHub Actions |
| `data/products/` | what a customer pays: every component and the total | GitHub Actions |
| [`data/checks.json`](data/checks.json) | what is wrong with all of the above | GitHub Actions |

## What a household pays

Start here. A product is what a customer pays: energy, grid, metering and fees together, excluding
VAT. `total.fixed` holds the prices per month, `total.windows` the prices per kWh by time of day.

| Product | Years | Stable URL |
|---|---|---|
| **Standard product**: Energy Blue + Einheitstarif | 2026, 2027 | `https://data.strompreise-schweiz.ch/bkw/products/standard/<year>.json` |

The standard product is what a household in the basic supply gets without choosing anything. It is a
proposal (`"status": "suggested"`) until checked against the BKW price sheet. The concession fees
depend on the municipality and are not included.

## Tariffs

| What | Stable URL |
|---|---|
| All tariffs of the current year | `https://data.strompreise-schweiz.ch/bkw/tariffs/current.json` |
| All tariffs of a given year | `https://data.strompreise-schweiz.ch/bkw/tariffs/<year>.json` |
| Day prices of a dynamic tariff, today and tomorrow | `https://data.strompreise-schweiz.ch/bkw/tariffs/current/<tariff>.json` |
| Day prices of a dynamic tariff, one day | `https://data.strompreise-schweiz.ch/bkw/tariffs/<year>/<tariff>/<date>.json` |

The yearly file is the VSE format (OpenAPI 1.0.1) with additional fields: `tariffs[].id`, `products`
and `_meta` (sources, repairs and corrections). For a dynamic tariff, `prices.dynamic.url` points to
its day prices. These are an addition to the yearly file: `tariffs/2026/<tariff>/` holds the days of
the tariff `<tariff>` in `tariffs/2026.json`.

The dynamic feed-in API of BKW belongs to the 2027 tariff `grid-sonne-scheint-plein-soleil-ne7`.
The yearly file for 2026 has no tariff with this API, so the days of 2026 are stored in
`data/sources/days/feed-in/` but not published; `checks.json` lists them under `withoutTariff`.

Timestamps are Swiss local time with an offset, for example `2026-11-02T00:00:00+01:00`.
Parse the offset and do not assume 96 intervals: a day has 92 in March and 100 in October.
Check `_meta.complete` before you use a day. The site serves the days of the current year; all years
are in git under `https://raw.githubusercontent.com/Strompreise-Schweiz/bkw/main/data/`.

Format details: [`SCHEMA.md`](https://github.com/Strompreise-Schweiz/tariff-core/blob/main/SCHEMA.md).

## License

See [`data/LICENSE`](data/LICENSE) for the data and [`LICENSE`](LICENSE) for the code.
