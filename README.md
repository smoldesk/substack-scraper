# Substack Scraper

![Substack Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/substack/banner.webp)

Scrape Substack newsletters: subscriber counts, paid tiers and prices, author socials and posts with likes and comments. No login, no API key. Runs on [Apify](https://apify.com/smoldesk/substack-scraper).

## What you get

- **Subscriber counts**: the public figure Substack shows, as a number and as the label (`1.2M+`).
- **Paid tiers**: whether paid is on, monthly and yearly price, currency, and the paid-subscriber label.
- **Author**: name, bio, X/Twitter link and Substack profile.
- **Posts**: title, date, paid or free, word count, likes, comments, restacks, authors and tags.
- **Full text**: optional `includePostContent` for free posts.
- **Discovery**: pull the top newsletters of a category such as `technology` or `finance`.

## Run it

Open the [Substack Scraper on Apify](https://apify.com/smoldesk/substack-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~substack-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "publications": ["lenny", "https://www.platformer.news"],
    "categories": ["technology"],
    "maxPublicationsPerCategory": 50,
    "maxPostsPerPublication": 20,
    "postsSince": "2026-10-01"
  }'
```

## Sample output

```json
{
  "type": "publication",
  "name": "Lenny's Newsletter",
  "url": "https://www.lennysnewsletter.com",
  "freeSubscriberCount": 1200000,
  "subscriberCountLabel": "1.2M+",
  "paidEnabled": true,
  "monthlyPrice": 20,
  "yearlyPrice": 200,
  "priceCurrency": "USD",
  "authorName": "Lenny Rachitsky",
  "authorTwitter": "https://x.com/lennysan",
  "firstPostDate": "2019-06-14T15:00:48.833Z"
}
```

```json
{
  "type": "post",
  "publicationName": "Lenny's Newsletter",
  "title": "How to tell if a startup has a moat",
  "publishedAt": "2026-10-13T12:45:41.160Z",
  "isPaid": true,
  "wordCount": 4153,
  "likes": 240,
  "restacks": 5,
  "tags": ["Career"]
}
```

Export as JSON, CSV or Excel, or pipe it into Google Sheets, Make, Zapier or n8n.

## Pricing

Pay per result. No monthly fee.

| Result | Per 1,000 |
|---|---|
| Newsletter | $3 |
| Post | $1 |

## Limits

- Subscriber counts are what Substack shows publicly. Some newsletters hide them.
- Paid posts return only the free preview text.
- Public data only. No emails and no private subscriber lists.

## FAQ

**Does Substack have an API?**
Not an official public one. This reads the pages and endpoints any visitor's browser loads and returns structured JSON.

**How do I see how many subscribers a Substack has?**
Add the newsletter to `publications` and keep `enrichPublications` on. You get `freeSubscriberCount` and `subscriberCountLabel`.

**Can I get subscriber emails?**
No. Subscriber lists are private. You get the public count and the author's public profile.

**How do I find newsletters in a niche?**
Set `categories` to a Substack category such as `finance` and `maxPublicationsPerCategory` to how many you want.

**How much does a newsletter charge for paid?**
`monthlyPrice` and `yearlyPrice` with `priceCurrency` on every newsletter record.

## More scrapers

Substack is one of 17 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
