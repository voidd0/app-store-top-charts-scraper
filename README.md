# App Store Top Charts Scraper: export to CSV or JSON with Python, Node.js or curl

Pull the Apple App Store top charts of paid apps and apps at no cost for any storefront country, up to 100 apps per chart. One row per (chart × country × app) with rank, name, developer, price, rating and number of ratings, genre, version and links, from Apple's open chart feed and lookup API.

This repository has ready scripts for the [App Store Top Charts Scraper: Ranks by Country](https://apify.com/v0iddo/app-store-top-charts) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/app-store-top-charts), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "chartTypes": [
    "topfreeapplications",
    "toppaidapplications"
  ],
  "countries": [
    "us",
    "gb"
  ]
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `chartTypes` | array | Which charts to read: `topfreeapplications` (apps at no cost) and `toppaidapplications` (paid apps). |
| `countries` | array | App Store storefront codes (ISO 3166-1 alpha-2), for example `us`, `gb`, `de`, `jp`, `br`. |
| `limit` | integer | How many apps to read per chart and country, 1 to 100 (the most Apple's open chart feed gives). |
| `withDetails` | boolean | Add each app's price, currency, rating, number of ratings, age rating, main genre, bundle id, version, last update and size from Apple's lookup API (one extr... |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "snapshotAt": "2026-11-09T02:40:50+00:00",
  "chart": "topfreeapplications",
  "country": "us",
  "rank": 1,
  "appId": "6741796873",
  "trackName": "TikTok Pro - Events",
  "artistName": "TikTok Ltd.",
  "genres": [],
  "releaseDate": "2026-06-03",
  "appUrl": "https://apps.apple.com/us/app/tiktok-pro-events/id6741796873",
  "artworkUrl": "https://is1-ssl.mzstatic.com/image/thumb/Purple211/v4/c7/e0/05/c7e00539-f686-7de1-776b-2389d4f8d3af/AppIcon_TikTok-0-0-1x_U007emarketing-0-8-0-85-220.png/100x100bb.png",
  "chartUpdatedAt": "2026-11-09T02:40:51+00:00",
  "price": 0,
  "currency": "USD",
  "rating": 4.82,
  "ratingCount": 88877,
  "contentRating": "12+",
  "primaryGenre": "Entertainment",
  "bundleId": "com.ss.iphone.ugc.tiktok.pro",
  "version": "47.6.0",
  "lastUpdated": "2026-11-03T03:44:23Z",
  "sizeMb": 1056.6
}
```

Fields: `snapshotAt`, `chart`, `country`, `rank`, `appId`, `trackName`, `artistName`, `genres`, `releaseDate`, `appUrl`, `artworkUrl`, `chartUpdatedAt`, `price`, `currency`, `rating`, `ratingCount`, `contentRating`, `primaryGenre`, `bundleId`, `version`, `lastUpdated`, `sizeMb`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/app-store-top-charts').call({"chartTypes": ["topfreeapplications", "toppaidapplications"], "countries": ["us", "gb"]});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~app-store-top-charts/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"chartTypes": ["topfreeapplications", "toppaidapplications"], "countries": ["us", "gb"]}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$0.10 per 1,000 results**, nothing per run. `limit` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
