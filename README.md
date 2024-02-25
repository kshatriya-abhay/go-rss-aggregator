# Tutorial link

[YouTube](https://youtu.be/un6ZyFkqFKo?si=Vw4tWm9h2m8OcWwc)
# Key Features
* Allows users to follow multiple feeds and fetch an aggregated list of posts.
* Scrapes the feeds automatically in a period manner.
* Multiple feeds are fetched in parallel (concurrency using Goroutines)
* User authentication - users are only access their own feeds using their *API Key*

# Installation

* Build using this command: ```go build```
* Depending on the operating system, it will either generate the output program exeutable called ```go-rss-aggregator``` or ```go-rss-aggregator.exe``` for windows.
* Start the server using ```./go-rss-aggregator``` or ```.\go-rss-aggregator.exe``` respectively.

# Usage

## Add User
Method: POST
Endpoint: /users
Sample Payload:
```json
{
    "name": "Abhay"
}
```
<details>
  <summary>Sample Response</summary>

```json
{
    "id": "2091380c-1702-458f-a0d0-c4476775ac9e",
    "created_at": "2024-02-25T10:50:35.946012Z",
    "updated_at": "2024-02-25T10:50:35.946012Z",
    "name": "Abhay",
    "api_key": "1609d1edc07a8692f8640e5eabd5574a2ef2c3dfd8f3a0ce732f494a42d57937"
}
```
</details>

## Get user data (authenticated)
Method: GET
Endpoint: /users
Payload is not required
Required header (sample):
```json
{
    "Authorization": "ApiKey c103ca32fee579530ef9abefc3894bb4ba7ddc2e5affb2e4e84497f062f5aa9f"
}
```
<details>
  <summary>Sample Response</summary>
  
  ```json
{
    "id": "2091380c-1702-458f-a0d0-c4476775ac9e",
    "created_at": "2024-02-25T10:50:35.946012Z",
    "updated_at": "2024-02-25T10:50:35.946012Z",
    "name": "Abhay",
    "api_key": "1609d1edc07a8692f8640e5eabd5574a2ef2c3dfd8f3a0ce732f494a42d57937"
}
```
</details>

## Create RSS Feed (authenticated)
Method: POST
Endpoint: /feeds
Sample Payload:
```json
{
    "name": "TechCrunch",
    "url": "https://techcrunch.com/feed/"
}
```
Required header (sample):
```json
{
    "Authorization": "ApiKey c103ca32fee579530ef9abefc3894bb4ba7ddc2e5affb2e4e84497f062f5aa9f"
}
```
<details>
  <summary>Sample Response</summary>

```json
{
    "id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3",
    "created_at": "2024-02-25T11:05:43.878948Z",
    "updated_at": "2024-02-25T11:05:43.878948Z",
    "name": "TechCrunch",
    "url": "https://techcrunch.com/feed/",
    "user_id": "5a551d87-c746-4960-9638-9b20c52a9348"
}
```
</details>

## Get All RSS Feeds
Method: GET
Endpoint: /feeds
Payload is not required
<details>
  <summary>Sample Response</summary>

```json
[
    {
        "id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3",
        "created_at": "2024-02-25T11:05:43.878948Z",
        "updated_at": "2024-02-25T11:05:43.878948Z",
        "name": "TechCrunch",
        "url": "https://techcrunch.com/feed/",
        "user_id": "5a551d87-c746-4960-9638-9b20c52a9348"
    },
    {
        "id": "946584e2-c7dc-45f4-896a-00631b6890ae",
        "created_at": "2024-02-19T11:40:06.135209Z",
        "updated_at": "2024-02-19T11:40:06.135209Z",
        "name": "WagsLane",
        "url": "https://www.wagslane.dev/index.xml",
        "user_id": "5a551d87-c746-4960-9638-9b20c52a9348"
    }
]
```
</details>

## Follow a Feed
Method: POST
Endpoint: /feed_follows
Sample Payload:
```json
{
    "feed_id" : "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
}
```
Required header (sample):
```json
{
    "Authorization": "ApiKey c103ca32fee579530ef9abefc3894bb4ba7ddc2e5affb2e4e84497f062f5aa9f"
}
```
<details>
  <summary>Sample Response</summary>

```json
{
    "id": "44d22f7b-e723-458b-88d7-7652f0e80a87",
    "created_at": "2024-02-25T15:29:01.294011Z",
    "updated_at": "2024-02-25T15:29:01.294011Z",
    "user_id": "5a551d87-c746-4960-9638-9b20c52a9348",
    "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
}
```
</details>

## Get the Feeds a User follows
Method: GET
Endpoint: /feed_follows
Payload is not required
Required header (sample):
```json
{
    "Authorization": "ApiKey c103ca32fee579530ef9abefc3894bb4ba7ddc2e5affb2e4e84497f062f5aa9f"
}
```
<details>
  <summary>Sample Response</summary>

```json
{
    "id": "44d22f7b-e723-458b-88d7-7652f0e80a87",
    "created_at": "2024-02-25T15:29:01.294011Z",
    "updated_at": "2024-02-25T15:29:01.294011Z",
    "user_id": "5a551d87-c746-4960-9638-9b20c52a9348",
    "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
}
```
</details>

## Unfollow a Feed
Method: DELETE
Endpoint: /feed_follows/{FeedFollowID}
FeedFollowID should be the ID fetched from GET /feed_follows
Payload is not required
Required header (sample):
```json
{
    "Authorization": "ApiKey c103ca32fee579530ef9abefc3894bb4ba7ddc2e5affb2e4e84497f062f5aa9f"
}
```
No response body will be sent, status code should be 200.
## Get Aggregated Feed

Method: `GET`
Endpoint: `/feed_follows`
Payload is not required
Required header (sample):
```json
{
    "Authorization": "ApiKey c103ca32fee579530ef9abefc3894bb4ba7ddc2e5affb2e4e84497f062f5aa9f"
}
```
<details>
  <summary>Sample Response</summary>

```json
[
    {
        "id": "cc0e18d2-d8f2-4902-97b1-5234acb8e4a2",
        "created_at": "2024-02-25T15:28:52.117929Z",
        "updated_at": "2024-02-25T15:28:52.117929Z",
        "title": "Europe remains hard to crack for North American GPs",
        "description": "<p>While North American VCs can see the potential value in backing European startups, it hasn't been easy for firms to launch strategies.</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-25T15:00:14Z",
        "url": "https://techcrunch.com/2024/02/25/europe-remains-hard-to-crack-for-north-american-gps/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "b3c5d4a1-e77e-41f1-91f5-151d4c27aaee",
        "created_at": "2024-02-25T15:28:52.123121Z",
        "updated_at": "2024-02-25T15:28:52.123121Z",
        "title": "Amba Kak creates policy recommendations to address AI concerns",
        "description": "<p>As a part of a multi-part series, TechCrunch is highlighting women innovators — from academics to policymakers — in the field of AI.</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-25T14:00:06Z",
        "url": "https://techcrunch.com/2024/02/25/amba-kak-creates-policy-recommendations-to-address-ai-concerns/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "1628affe-a7ea-40a1-aa62-9f4259c0aa88",
        "created_at": "2024-02-25T11:06:14.290498Z",
        "updated_at": "2024-02-25T11:06:14.290498Z",
        "title": "Google releases new open LLMs, Rivian lays off staff and Signal rolls out usernames",
        "description": "<p>Welcome, folks, to Week in Review (WiR), TechCrunch’s regular newsletter covering noteworthy happenings in the tech industry. This week, Google launched two new open large language models, Gemma 2B and Gemma 7B, in its continued bid for generative AI dominance. The company, which describes the LLMs as &#8220;inspired by Gemini,&#8221; its flagship family of GenAI [&#8230;]</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T21:15:00Z",
        "url": "https://techcrunch.com/2024/02/24/google-releases-new-open-ai-models-layoffs-at-rivian-and-signal-rolls-out-usernames/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "78184e4c-3a3d-4dd9-93a8-9874d8d32a03",
        "created_at": "2024-02-25T11:06:14.292401Z",
        "updated_at": "2024-02-25T11:06:14.292401Z",
        "title": "With liquidity rare, VCs may get creative to return investor cash",
        "description": "<p>Welcome to the very last issue of The Exchange! With TechCrunch+ sunsetting this month, The Exchange column and its newsletter are also coming to an end. Thank you for reading, emailing, tweeting, and hanging out with us for so many years. P.S. A special thanks from myself to Anna, who was nothing short of a [&#8230;]</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T18:05:20Z",
        "url": "https://techcrunch.com/2024/02/24/with-liquidity-rare-vcs-may-get-creative-to-return-investor-cash/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "8e12349f-8c13-4602-806b-c1322265793b",
        "created_at": "2024-02-25T11:06:14.293757Z",
        "updated_at": "2024-02-25T11:06:14.293757Z",
        "title": "Equity Shot: All about the Reddit IPO!",
        "description": "<p>﻿ Listen here or wherever you get your podcasts. Hello, and welcome to Equity, a podcast about the business of startups, where we unpack the numbers and nuance behind the headlines. Weekend team, we have something both short and sweet for you: A dig into the Reddit IPO filing that came out just after we recorded our [&#8230;]</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T18:05:00Z",
        "url": "https://techcrunch.com/2024/02/24/equity-shot-all-about-the-reddit-ipo/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "af60cb51-641c-4f31-94bc-329714041905",
        "created_at": "2024-02-25T11:06:14.294373Z",
        "updated_at": "2024-02-25T11:06:14.294373Z",
        "title": "Stellantis CEO says there’s still life in Waymo deal for self-driving delivery vans",
        "description": "<p>Stellantis, the automaker that owns 14 brands including Chrysler, Jeep and Ram, and autonomous vehicle technology company Waymo are not only still working together, the companies are deepening the partnership, CEO Carlos Tavares told TechCrunch in a recent interview. This &#8220;deepened&#8221; partnership will focus on commercial self-driving Ram delivery vans, a target that was first [&#8230;]</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T16:00:01Z",
        "url": "https://techcrunch.com/2024/02/24/stellantis-ceo-says-theres-still-life-in-waymo-deal-for-self-driving-delivery-vans/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "d8d5c483-db7a-4b49-a49f-fa65aa72a1d0",
        "created_at": "2024-02-25T11:06:14.294538Z",
        "updated_at": "2024-02-25T11:06:14.294538Z",
        "title": "This Week in AI: Addressing racism in AI image generators",
        "description": "<p>Keeping up with an industry as fast-moving as AI is a tall order. So until an AI can do it for you, here’s a handy roundup of recent stories in the world of machine learning, along with notable research and experiments we didn’t cover on their own. This week in AI, Google paused its AI chatbot Gemini&#8217;s [&#8230;]</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T14:45:12Z",
        "url": "https://techcrunch.com/2024/02/24/this-week-in-ai-addressing-racism-in-ai-image-generators/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "2e0c5408-3b6e-4217-9221-9ae275577f84",
        "created_at": "2024-02-25T11:06:14.295166Z",
        "updated_at": "2024-02-25T11:06:14.295166Z",
        "title": "Miranda Bogen is creating solutions to help govern AI",
        "description": "<p>TechCrunch has launched a series of interviews focusing on remarkable women who’ve contributed to the AI revolution.</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T14:00:13Z",
        "url": "https://techcrunch.com/2024/02/24/miranda-bogen-is-creating-solutions-to-help-govern-ai/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "f8429024-14d6-4ac8-b3b7-e0116d51f3d9",
        "created_at": "2024-02-25T11:06:14.295463Z",
        "updated_at": "2024-02-25T11:06:14.295463Z",
        "title": "Byju’s founder, ousted by shareholders, insists he is still the CEO",
        "description": "<p>Byju Raveendran, the founder of eponymous edtech group Byju&#8217;s, told employees on Saturday that he continues to remain the chief executive of the startup and that rumors of his firing have been &#8220;greatly exaggerated,&#8221; a day after a shareholder group voted to remove him at an emergency general meeting. In a 758-word letter, content of [&#8230;]</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T13:39:51Z",
        "url": "https://techcrunch.com/2024/02/24/byjus-founder-ousted-by-shareholders-says-rumors-of-his-firing-greatly-exaggerated/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    },
    {
        "id": "c8094737-1c8d-4179-a764-6eb62cc3c94c",
        "created_at": "2024-02-25T11:06:14.296144Z",
        "updated_at": "2024-02-25T11:06:14.296144Z",
        "title": "Quick thinking and a stroke of luck averted a moon lander disaster for Intuitive Machines",
        "description": "<p>Intuitive Machines&#8217; spacecraft touched down yesterday on the lunar surface . . . sideways. CEO Steve Altemus confirmed during a press conference Friday that, while it wasn&#8217;t a perfect landing, it&#8217;s nothing short of a miracle the spacecraft landed intact at all. Using a small model of the lander, Altemus demonstrated how engineers believe the spacecraft, called Odysseus, made its [&#8230;]</p>\n<p>© 2024 TechCrunch. All rights reserved. For personal use only.</p>\n",
        "published_at": "2024-02-24T00:32:49Z",
        "url": "https://techcrunch.com/2024/02/23/quick-thinking-and-a-stroke-of-luck-averted-a-moon-lander-disaster-for-intuitive-machines/",
        "feed_id": "3728502d-b193-4ed6-b9a2-d8a1cccb1bc3"
    }
]
```
</details>
