# Substack Newsletter Sponsorship Prospect Finder

Substack publications ranked for sponsorship: subscriber count, paid-tier size, plan prices, posting cadence and engagement per 1,000 subscribers.

[![Run on Apify](https://img.shields.io/badge/Run%20on-Apify-0f9f74)](https://apify.com/datagrit/substack-sponsor-prospector) [![Docs](https://img.shields.io/badge/docs-getdatagrit.github.io-0e1726)](https://getdatagrit.github.io/substack-sponsor-prospector/)

**from $3.15 per 1,000 results + $10 per run (pay per result; the rate depends on your Apify plan).** Export as JSON, CSV or Excel, call it through the API, or schedule it on Apify.

## What it does

Substack Newsletter Sponsorship Prospect Finder turns Substack category rankings, a list of newsletters you name, or the recommendation links of a newsletter into one analyzed row per publication. Each row combines the public profile (subscriber count, paid-subscriber label, monthly, annual and founding prices) with numbers measured from the publication's own post archive: posts in the last 30 and 90 days, posts per week, the share of paid-only posts, median reactions and comments per post, reactions per 1,000 subscribers, and how many newsletters started recommending it in the last 30 days.
Export the result as JSON, CSV or Excel, call it through the Apify API, or plug it into n8n, Make and AI agents through MCP.

## Quick start

1. Open [Substack Newsletter Sponsorship Prospect Finder on Apify Store](https://apify.com/datagrit/substack-sponsor-prospector) and click **Try for free**.
2. Fill in the input form (or paste the JSON below) and run it.
3. Download the dataset, or fetch it from the API.

```json
{
  "source": "leaderboard",
  "categories": [
    "technology",
    "business"
  ],
  "maxPerCategory": 10,
  "maxItems": 20
}
```

## Input

| Field | Type | What it does |
|---|---|---|
| `source` | string | Where the publications come from. "leaderboard" reads the Substack category rankings, "publications" analyzes the publications you list, "lookalikes" finds publications that the ones you list recommend. |
| `categories` | array | Leaderboard categories to read when the source is "leaderboard". Available: culture, technology, business, us-politics, finance, food, sports, art, world-politics, health-politics, news, fashionandbeauty, music, faith, climate, science, literature, fiction, health, design, travel, parenting, philosophy, comics, international, crypto, history, humor, education, film-and-tv, home-garden, games. |
| `leaderboardType` | string | Which ranking to read: "all" is the ranking of all publications of the category, "paid" is the ranking of publications with paid subscriptions. |
| `maxPerCategory` | integer | How many leaderboard positions to scan per category before filters apply. One page of the ranking holds 25 positions. |
| `publications` | array | Used when the source is "publications": Substack URLs, custom domains or subdomains such as "lenny". |
| `lookalikesOf` | array | Used when the source is "lookalikes": publications whose recommendations are followed to find similar newsletters. Accepts URLs, custom domains or subdomains. |
| `minSubscribers` | integer | Keep publications with at least this many displayed subscribers. Zero disables the filter. Publications that hide their count are dropped when a limit is set. |
| `maxSubscribers` | integer | Keep publications with at most this many displayed subscribers. Zero disables the filter. Publications that hide their count are dropped when a limit is set. |
| `requirePaidPlan` | boolean | Keep only publications that have paid subscriptions switched on and a price. |
| `maxMonthlyPriceUsd` | number | Keep publications whose regular monthly plan costs at most this many US dollars. Zero disables the filter. Publications without a monthly plan are dropped when a limit is set. |
| `language` | string | Keep publications whose language code starts with this value, for example "en" or "de". Empty keeps all languages. |
| `activeWithinDays` | integer | Keep publications whose newest post is at most this many days old. Zero disables the filter. |
| `minPosts30d` | integer | Keep publications that published at least this many posts in the last 30 days. Zero disables the filter. |
| `maxItems` | integer | Stop after this many publications in total. |
| `proxyConfiguration` | object | Optional proxy. Leave disabled unless Substack blocks datacenter traffic; residential proxy raises the platform cost of the run. |

## Output

| Field | Type | Description |
|---|---|---|
| `found` | boolean | False only on status rows, which explain why a requested publication produced no result. Status rows are not billed. |
| `input` | string | Publication reference or source that a status row refers to. Empty on result rows. |
| `note` | string | Reason for a status row. Empty on result rows. |
| `publicationId` | integer | Substack publication ID, unique and stable. |
| `name` | string | Publication name. |
| `subdomain` | string | Substack subdomain of the publication. |
| `customDomain` | string | Custom domain of the publication, when it has one. |
| `url` | string | Home page of the publication. |
| `description` | string | Short description written by the publisher. |
| `language` | string | Language code of the publication as set by the publisher. |
| `authorName` | string | Name of the main author as shown on Substack. |
| `authorHandle` | string | Substack handle of the main author. |
| `authorBio` | string | Public bio of the main author. |
| `authorUrl` | string | Substack profile page of the main author. |
| `subscribers` | integer | Subscriber count as Substack displays it, rounded by Substack (for example 318,000). Null when the publisher hides it. |
| `paidSubscribersLabel` | string | Order of magnitude of paid subscribers as a band label. Substack does not publish an exact number. Taken from the publication's own band (the numeric order of magnitude in its record, which matches the ranking label when one is shown). The author's bestseller tier fills in only when the publication has no band of its own, and never overrides it (see paidSubscribersSource). Null when the publication's own band is 0 or absent and the author has no bestseller badge, which does not mean it has no paid subscribers. |
| `paidSubscribersAtLeast` | integer | Lower bound implied by the band: 1 (single digits), 10, 100, 1000, 10000, 100000 or 1000000. Null when paidSubscribersLabel is null. |
| `paidSubscribersSource` | string | Where the paid-subscriber band comes from: ranking-label (the publication's label, which equals the numeric order of magnitude in the same record), ranking-order-of-magnitude (the numeric order of magnitude of the publication when its label is not shown in that context) or author-bestseller-tier (the author's bestseller badge, used only when the publication has no band of its own; set per author, so it can sit one step above or below the publication). Null when there is no band. |
| `bestsellerTier` | integer | Substack bestseller badge of the author: 0 (none), 100, 1000 or 10000 paid subscribers. Set per author, so it can differ from the band of the publication in paidSubscribersAtLeast; it never overrides that band. |
| `paymentsState` | string | Whether paid subscriptions are switched on, as reported by Substack (enabled, disabled or not set up). |
| `monthlyPriceUsd` | number | Regular monthly subscription price in US dollars. Null when there is no monthly plan. |
| `annualPriceUsd` | number | Regular annual subscription price in US dollars. Null when there is no annual plan. |
| `foundingPriceUsd` | number | Price of the founding member plan in US dollars. Null when the publication has none. |
| `annualDiscountPercent` | integer | Discount of the annual plan against twelve monthly payments. Null when either plan is missing. |
| `firstPostAt` | string | ISO timestamp of the first published post. |
| `hasPodcast` | boolean | Whether the publication publishes a podcast. |
| `hasCommunity` | boolean | Whether the community feature is switched on. |
| `foundVia` | string | How the publication was found: leaderboard, input or lookalike. |
| `category` | string | Leaderboard category the publication was found in. Null when found via input or lookalike. |
| `rank` | integer | Position in the category leaderboard, 1 = top. Null when not found via leaderboard. |
| `lookalikeOf` | string | Name of the seed publication whose recommendations led to this one. Null otherwise. |
| `lastPostAt` | string | ISO timestamp of the newest post, newsletter threads excluded. |
| `daysSinceLastPost` | integer | Whole days between the newest post and the run. |
| `posts30d` | integer | Posts published in the 30 days before the run. |
| `posts90d` | integer | Posts published in the 90 days before the run. |
| `postsPerWeek` | number | Posting cadence over the last 90 days, or over the age of the publication when it is younger. |
| `paidPostShare90d` | number | Share of posts of the last 90 days that are not open to everyone, between 0 and 1. |
| `medianReactionsPerPost` | number | Median likes per post over posts older than three days within the last 90 days. |
| `medianCommentsPerPost` | number | Median comments per post over posts older than three days within the last 90 days. |
| `reactionsPer1kSubscribers` | number | Median reactions per post divided by the displayed subscriber count, times 1,000. Null when either is unknown. |
| `recommendedBy30d` | integer | Publications that started recommending this one in the last 30 days. |
| `recommendedBy30dCapped` | boolean | True when all 50 newest recommendations fall within 30 days, so the real count is at least recommendedBy30d. |
| `recommendedByLatestAt` | string | ISO timestamp of the newest recommendation received from another publication. |
| `dataGaps` | array | Incomplete parts of this row. recommendations: the recommendation list could not be read, so recommendedBy30d, recommendedBy30dCapped and recommendedByLatestAt are null. archive-truncated: the post archive was read up to its 400-post cap without reaching 90 days back, so the activity counts are lower bounds. A publication whose post archive cannot be read at all is skipped without billing and never appears here, so archive is not a value of this list. |
| `sourceUrl` | string | Page the record was read from. |
| `scrapedAt` | string | ISO timestamp of the run. |

Sample record:

```json
{
  "found": true,
  "input": "no-such-newsletter.substack.com",
  "note": "No Substack publication was found at no-such-newsletter.substack.com.",
  "publicationId": 6349492,
  "name": "SemiAnalysis",
  "subdomain": "semianalysis",
  "customDomain": "newsletter.semianalysis.com",
  "url": "https://newsletter.semianalysis.com",
  "description": "Bridging the gap between the world's most important industry, semiconductors, and business.",
  "language": "en",
  "authorName": "Dylan Patel",
  "authorHandle": "semianalysis",
  "authorBio": "Bridging the gap between business and the worlds most important industry.",
  "authorUrl": "https://substack.com/@semianalysis",
  "subscribers": 318000,
  "paidSubscribersLabel": "Thousands of paid subscribers",
  "paidSubscribersAtLeast": 1000,
  "paidSubscribersSource": "ranking-label",
  "bestsellerTier": 1000,
  "paymentsState": "enabled",
  "monthlyPriceUsd": 50,
  "annualPriceUsd": 500,
  "foundingPriceUsd": 1000,
  "annualDiscountPercent": 17,
  "firstPostAt": "2020-05-22T21:26:00.000Z",
  "hasPodcast": false,
  "hasCommunity": true,
  "foundVia": "leaderboard",
  "category": "technology",
  "rank": 1,
  "lookalikeOf": "Lenny's Newsletter",
  "lastPostAt": "2026-09-29T14:02:11.000Z",
  "daysSinceLastPost": 2,
  "posts30d": 9,
  "posts90d": 31,
  "postsPerWeek": 2.4,
  "paidPostShare90d": 0.35,
  "medianReactionsPerPost": 412,
  "medianCommentsPerPost": 38,
  "reactionsPer1kSubscribers": 1.3,
  "recommendedBy30d": 4,
  "recommendedBy30dCapped": false,
  "recommendedByLatestAt": "2026-09-27T08:15:00.000Z",
  "dataGaps": [],
  "sourceUrl": "https://substack.com/api/v1/category/public/4/all?page=0",
  "scrapedAt": "2026-10-01T09:30:00.000Z"
}
```

## Call it from code

Runnable examples are in [`examples/`](examples). Replace `YOUR_APIFY_TOKEN` with the token from your Apify account settings.

```bash
curl -X POST "https://api.apify.com/v2/acts/datagrit~substack-sponsor-prospector/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"source":"leaderboard","categories":["technology","business"],"maxPerCategory":10,"maxItems":20}'
```

## FAQ

**How fresh is the data?**  
Every run reads Substack live. Ranks change between runs, so the same leaderboard input can return slightly different publications.

**How is the posting cadence calculated?**  
From the post archive, over the last 90 days; for publications younger than 90 days the window is their age. Reactions are medians over posts older than three days, so a fresh post does not distort them.

**Why is a number empty?**  
Either it could not be read in this run, which `dataGaps` and the run summary name, or Substack shows no figure for that publication. An empty `paidSubscribersLabel` means the publication's record carries no paid-subscriber band (its order of magnitude is 0 or absent) and the author has no bestseller badge to fall back on; it does not mean the publication has no paid subscribers.

**What happens when Substack rate-limits the run, or one newsletter's site stops answering?**  
The Actor retries failed requests with a growing pause and follows the `Retry-After` header, but the time a run loses to failures is limited in three layers: 30 seconds per newsletter address (the pauses between attempts plus the time spent in attempts that failed or timed out; each attempt waits at most 25 seconds for an answer), 75 seconds for Substack's own ranking and recommendation service, and 150 seconds of failed-request time for the whole run, counted as a sum over requests that run in parallel, so it is used up in less than 150 seconds of clock time. Healthy requests never count. One newsletter whose site does not answer therefore costs at most about 30 seconds of that budget and affects only that newsletter. Newsletters that have not failed yet are protected from the others: once the run budget is used up, every address that has not failed in this run still gets one attempt of up to 8 seconds per request, and an address that has failed gets no further request and no pause that no longer fits. When a post archive stays unread, the publication is skipped without billing and, in `publications` mode, gets a status row. A post archive or recommendation list that arrives as something other than JSON (for example an HTML error page) is treated like any other failed read of that one newsletter: a publication without a readable archive is skipped without billing, one without a readable recommendation list gets `dataGaps: ["recommendations"]`, and neither ends the run. A rate-limited run, or a source that accepts connections and never answers, therefore ends in minutes, not hours. The run summary and the status rows say which case it was: a request that failed is reported as a source or network problem, a publication for which no request was sent because the budget was already used up is reported as a limit of the run, not as a problem of the address. The run fails with a clear message in three cases only: most post archives or recommendation lists that were answered with an error or an unreadable body (judged from at least 5 answers, so a few silent sites at the start of a ranking do not end the run), 12 or more archive requests that got no answer at all and not one that worked, or, at the end of the run, no post archive readable at all. Addresses that do not answer are never counted as a sign that the source changed. An address whose domain does not exist is reported at once, without retries.

**Can I schedule runs?**  
Yes, use Apify schedules or call the Actor from your own workflow.

**Something looks wrong.**  
Open an issue with the input you used; layout changes at the source are fixed quickly.

## More from datagrit

- [Bilibili Anime Catalog, Rankings & Calendar](https://github.com/getdatagrit/bilibili-anime-series-tracker) - Bilibili anime, Chinese animation, film, documentary and TV series: filterable catalog, Top 100 rankings, release calendar and season details with ratings and follower counts.
- [Bluesky Community Finder: Starter Packs & Feeds](https://github.com/getdatagrit/bluesky-community-finder) - Find Bluesky starter packs, custom feeds and curated lists by topic, with join counts, feed likes and full member lists with follower counts.
- [Medium Publication Finder: Subscribers & Activity](https://github.com/getdatagrit/medium-publication-finder) - Find Medium publications by keyword and score each one: subscribers, posting cadence, claps per post, paywalled share and top authors.
- [Telegram Channel Analytics: Growth and Reach](https://github.com/getdatagrit/telegram-channel-growth-analytics) - One row per public Telegram channel: subscribers and growth since your last run, median views, view rate, posting cadence, reactions and forward sources.
- [TED Contract Expiry Radar - Recompete Leads](https://github.com/getdatagrit/ted-contract-expiry-radar) - Find EU public contracts approaching expiry from TED award notices: incumbent, buyer, value, end date and renewal options.

All Actors: [https://getdatagrit.github.io/](https://getdatagrit.github.io/) · [Apify Store](https://apify.com/datagrit)

---

This repository holds documentation and usage examples. Questions, bug reports and feature requests: use the **Issues** tab of the Actor page on [Apify Store](https://apify.com/datagrit/substack-sponsor-prospector). Examples are MIT licensed.
