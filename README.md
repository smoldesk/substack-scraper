# Substack Scraper

![Substack Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/substack/banner.webp)

Scrape Substack newsletters: subscriber counts, paid tiers and prices, author socials, posts with likes, comments and full text, the comment threads under each post, keyword search and Notes. No login, no API key. Runs on [Apify](https://apify.com/smoldesk/substack-scraper).

## What you get

- **Subscriber counts**: the public figure Substack shows, as a number and as the label (`1.2M+`).
- **Subscriber change**: schedule the Actor and every newsletter shows the numbers from your previous run and the change.
- **Paid tiers**: whether paid is on, monthly and yearly price, currency, and the paid-subscriber label.
- **Author**: name, bio, follower count, X/Twitter and all profile links, plus contact emails and social links from the About page when the newsletter publishes them.
- **Posts**: title, date, paid or free, word count, likes, comments, restacks, authors, tags and posting stats (average likes, posts a month).
- **Full text** as clean text and Markdown with `includePostContent` (paid posts: free preview only).
- **Comments and replies** under each post, one row each, with the thread position (`parentCommentId`, `depth`).
- **Who liked a post** with `includeLikers`.
- **Discovery**: top newsletters of a category such as `technology` or `finance`, or a keyword search across all of Substack.
- **Notes**: from a keyword search or from any profile.

## Run it

Open the [Substack Scraper on Apify](https://apify.com/smoldesk/substack-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~substack-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "publications": ["lenny", "https://www.platformer.news"],
    "maxPostsPerPublication": 20,
    "postsSince": "2026-10-01",
    "includeComments": true,
    "maxCommentsPerPost": 50
  }'
```

Find newsletters instead of pasting them: `"categories": ["technology"]`, or `"searchQueries": ["newsletter growth"]` with `"searchNotes": true`. Latest Notes of a writer: `"notesFrom": ["lenny"]`.

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
  "authorFollowerCount": 1395371,
  "firstPostDate": "2019-06-14T15:00:48.833Z",
  "contactEmails": [],
  "stats": {"postsAnalyzed": 20, "avgLikes": 471.1, "avgComments": 10.2, "postsPerMonth": 3.1, "paidPostShare": 0.8},
  "previousSubscriberCount": 1200000,
  "subscriberCountChange": 0
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

```json
{
  "type": "comment",
  "postTitle": "Jensen is reportedly about to get fired",
  "publicationName": "Marcus on AI",
  "parentCommentId": null,
  "depth": 0,
  "text": "If he is, then good. Time to hold our leaders both political and economic to higher ethical standards.",
  "likes": 30,
  "replyCount": 0,
  "authorName": "Goose of Truth"
}
```

Replies come right after their parent, with `parentCommentId` and `depth` (1 = reply, 2 = reply to a reply). Export as JSON, CSV or Excel, or pipe it into Google Sheets, Make, Zapier or n8n.

## Pricing

Pay per result. No monthly fee. Prices drop on paid Apify plans.

| Result (per 1,000) | Free | Starter | Scale | Business |
|---|---|---|---|---|
| Newsletter (with subscriber count, prices and author socials) | $3.00 | $3.00 | $2.60 | $2.25 |
| Post | $1.00 | $1.00 | $0.90 | $0.75 |

Comments and replies cost **$0.50 per 1,000** from 26 Nov 2026 and are free until then. Notes are free for now; a price of $0.50 per 1,000 is planned. Contact emails, posting stats, subscriber tracking and likers cost nothing extra.

## Limits

- Subscriber counts are what Substack shows publicly, as ranges such as "Over 266,000 subscribers". `freeSubscriberCount` is that lower bound. Some newsletters hide the count.
- Paid posts return only the free preview text.
- Public data only. No private subscriber lists. Contact emails appear only when the newsletter publishes them on its About page.

## FAQ

**Does Substack have an API?**
Not an official public one. This reads the pages and endpoints any visitor's browser loads and returns structured JSON.

**How do I see how many subscribers a Substack has?**
Add the newsletter to `publications` and keep `enrichPublications` on. You get `freeSubscriberCount` and `subscriberCountLabel`.

**Can I get subscriber emails?**
No. Subscriber lists are private. You get the public count, the author's public profile and any contact email the newsletter published on its About page.

**How do I find newsletters in a niche?**
Set `categories` to a Substack category such as `finance`, or use `searchQueries` for a keyword search across all of Substack. Turn on `scrapeFoundPublications` to also get a newsletter row for each newsletter the search finds.

**Can I scrape Substack comments?**
Set `includeComments` to `true`. Every comment and reply is its own row, best or newest first with `commentsSort`.

**Can I track a newsletter's growth?**
Schedule the Actor with the same newsletters. With `trackSubscribers` on (the default), each row shows the previous subscriber numbers and the change.

**How much does a newsletter charge for paid?**
`monthlyPrice` and `yearlyPrice` with `priceCurrency` on every newsletter record.

## More scrapers

Substack is one of 20 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
