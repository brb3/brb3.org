---
title: Podcasting and Cryptocurrency
author: Bobby Burden III
date: '2023-11-14'
categories:
  - podcasting
  - cryptocurrency
  - decentralization
---

The modern world of Podcasts is very different from the original idea that lead to their creation.
In a time when the web was much more decentralized, RSS was a valuable tool to connect creators with consumers.
Finding an interesting blog or news site that published an RSS feed made it much easier to read their content
especially in a time when dial-up was still very much a thing for many internet users. I remember, back in the early
2000s, subscribing to RSS feeds and dialing in long enough to refresh my RSS reader and then disconnecting again to read
through everything it had grabbed for me.

At that time, transferring large video or audio files across the web was much more time consuming than it is now.
If there was a way to marry these two things together, a simple RSS feed and _multimedia_, more people would be able
to access content without time consuming downloads or tying up phone lines.

# So What Even is RSS?
Really Simple Syndication, or RSS[^1], is an XML document that provides a computer-readable way to check for newly
published content. A simple RSS file might look like this:

```xml
<rss version="2.0">
    <channel>
        <title>brb3.org</title>
        <link>https://brb3.org/</link>
        <item>
            <title>Podcasting and Cryptocurrency</title>
            <link>https://brb3.org/post/2023/11/14/podcasting-and-cryptocurrency/</link>
            <description>
                The modern world of Podcasts is very different from the original idea that lead to their creation.
            </description>
        </item>
    </channel>
</rss>
```

This can be generated automatically by the site's software (like [Hugo](https://gohugo.io/) does for
[this site](/index.xml)) and easily consumed by an application that can parse XML.  The entire article contents can be
included, or just a description (like you see here). Additionally, and this is where the magic for podcasts happen, a
_media_ file can be included with an `<enclosure>` tag.

Through this mechanism, just a simple XML file, a user's power on the Web is greatly increased by an RSS Reader (RIP
Google Reader). No longer does a user need to check 10 sites every morning for new content, or scour through Reddit
boards, or check YouTube, or anything else. The act of opening the RSS Reader and telling it to fetch all your feeds can
quickly give you an overview of what's new, and what you might want to read.

I understand that in a time where driving site traffic for advertisers is seen as more important than providing good
content that allowing the user to _check_ without _visiting_ is a bad thing. But in this post I hope to explain how
now, through a Value4Value system (more on this later) and the building blocks set in place over 20 years ago, we can
reclaim this better Web.

# The Tale of the `<enclosure>`
Back to that `<enclosure>` tag. In 2000, [Adam Curry](https://en.wikipedia.org/wiki/Adam_Curry) described a problem
to [Dave Winer](https://en.wikipedia.org/wiki/Dave_Winer) and set in motion the changes that eventually created
podcasting.

From my understanding, the problem was described as the "click-wait system". Where a user wants to watch a video, so
they click... and they wait... And then they can watch the video. Again, at a time when internet connectivity speeds
were much slower, this could mean several minutes of waiting for a video that's very short and low quality.

But adding an `<enclosure>` tag to RSS would mean that an `<item>` could describe media files.
```xml
    <item>
        <title>brb3 podcast Episode 1</title>
        <link>https://example.com/brb3/episode1</link>
        <enclosure url="https://example.com/brb3/episode1.mp3" length="12345" type="audio/mpeg" />
        <description>The first episode of my pretend podcast that doesn't exist</description>
    </item>
```

Now your RSS reader (or a purpose made podcast application), can fetch those files for you _before_ you know you want
them. It could be scheduled to check periodically in the background or at a time of day when you are not likely to be
using your internet connection. Then you click and are immediately listening or watching or whatever the media file
contains.

Today, this is how Podcast apps work. If you have Podverse installed on your phone, for example, when it refreshes
your subscriptions it is just reading RSS files and presenting you with the results.

# Advertisers and Podcasting: It's a profit deal
![It's a profit deal!](/profit_deal.gif)

If you currently listen to podcasts, you'll know that the overwhelming majority are monetized through advertising.
It's hard to listen to a podcast (or watch a YouTube video for that matter) without hearing about:
- Some fancy new underwear!
- An electric razor you must have or no one will love you
- AUDIBLE
- Generic VPN service #82

Advertising [makes us unhappy](https://hbr.org/2020/01/advertising-makes-us-unhappy),
[causes creators to self-censor](https://www.liberties.eu/en/stories/self-censorship/43569), and
[drives consumerism](https://2012books.lardbucket.org/books/business-ethics/s16-03-we-buy-therefore-we-are-consum.html).

The listeners of these podcasts are constantly bombarded by these advertisements to the point that they probably don't
even hear them anymore. They are just subconsciously consumed and live rent-free. Imagine if the content creator's
words and ideas were their own. If the consequence of free speech wasn't dictated by brand safety guidelines but instead
by the audience of the creator.

This happens now in many places, but most often on places like [Twitch](https://www.twitch.tv/) in a limited way. Twitch
does sell advertising on their platform, but users can _subscribe_ to a Twitch creator (for $5/mo) so that they don't
see advertising on that creator's stream. This provides direct funding from the consumer -> Twitch -> creator. There are
still guidelines of what is acceptable on Twitch, but a creator doesn't have to worry about accidentally saying they
prefer Pepsi over Coke, or some other cardinal sin that could threaten their livelihood simply by mentioning the wrong
company or otherwise stepping outside the narrow band of acceptable opinions that an advertiser wishes to be associated
with.

In the case of Twitch: Twitch is providing a service through both hosting a stream (and all the technical problems that
come with it) but also providing discoverability of streams for the consumer. In a more idealistic Web, the streamer
should be able to stream without a corporate entity between them and their subscribers (which also takes ~50% of
subscription revenue). But that's a tangent for another blog post (in 3 more years, probably).

# Podcasting 2.0

Revisiting our earlier character of Adam Curry, he currently is working with two different initiatives that bring this
all together for podcasts:
1. [Podcast Index](https://podcastindex.org/) - A free, searchable index of millions of podcasts
2. [Value4Value](https://value4value.info/) - A monetization avenue for creators

The Podcast Index provides a way for content creators to easily add their feed to a public index. Developers can then
query the feed[^2] to provide users with a way to quickly find podcasts that are relevant to their interests.

With Value4Value, the concept is to ask the consumer (the listener, the viewer, etc) to give back value for the _value_
they receive by consuming the creator's content. This works. Consumers _want_ to pay for something, but can't always do
so. If you give it away and ask for some kind of value in return (money primarily, but also expertise, etc) some
consumers will respond by paying. Granted, the amount of consumers that do this is going to be _small_, but compared to
advertising CPM, this could result in more value per consumer than the "traditional" advertising monetization route.

(Sidebar: Twitch provides a way for viewers to directly donate to a creator via "bits", which is halfway there)

Where these two pieces collide is in Podcasting 2.0. Through a namespace maintained by Podcast Index, the RSS spec is
extended to provide additional podcast-specific tags[^3]. A compliant podcast client application can now pay attention
to a `<podcast:value>` tag in the RSS feed, for example (from the docs):
```xml
<podcast:value
    type="lightning"
    method="keysend"
    suggested="0.00000005000"
></podcast:value>
```

This tells the client that the podcast accepts _value_ from the listener via the Lightning network (a Bitcoin network)
with a suggested amount of 0.00000005000 BTC (or 5 Satoshi, $0.0018 USD at time of writing). Inside that tag, recipients
can be added (again, from the docs):

```xml
  <podcast:valueRecipient
      name="Alice (Podcaster)"
      type="node"
      address="02d5c1bf8b940dc9cadca86d1b0a3c37fbe39cee4c7e839e33bef9174531d27f52"
      split="40"
  />
  <podcast:valueRecipient
      name="Bob (Podcaster)"
      type="node"
      address="032f4ffbbafffbe51726ad3c164a3d0d37ec27bc67b29a159b0f49ae8ac21b8508"
      split="40"
  />
  <podcast:valueRecipient
      name="Carol (Producer)"
      type="node"
      address="02dd306e68c46681aa21d88a436fb35355a8579dd30201581cefa17cb179fc4c15"
      split="15"
  />
  <podcast:valueRecipient
      name="Hosting Provider"
      type="node"
      address="03ae9f91a0cb8ff43840e3c322c4c61f019d8c1c3cea15a25cfc425ac605e61a4a"
      split="5"
      fee="true"
  />
```

Now, if you have a wallet connected to your podcast client, you can "boost" (a word used to describe a donation or tip)
the creators directly from your wallet. If you really liked this episode and want to give $1 USD, that's as simple as
pressing a button in the podcast client, entering the amount (~2,800 Satoshi) and sending it. The podcast client takes
care of the work of setting up the proper transactions (using the `address` and `split` values) on your behalf.

Once you get past the initial friction of subscribing to the feed, which is made easier with Podcast Index, and have a
wallet connected to your podcast client[^4], it is very easy to boost podcasts.

# The missing piece
Ok, I'm trying to tie this all together with an idea that I've been chewing on that relies on a resurgence of RSS feeds
across the web.

If we expanded this specification out a bit more to allow a `<channel>` to have a `<channel:value>`, we could provide
this same Value4Value model to written articles or news stories, or whatever. Imagine reading this article in and RSS
reader and saying "well at least he tried" and throwing 5 Satoshi my way. But expand that out further.

Imagine your local newspaper published an RSS feed and you could read the articles and configure your RSS Reader to
"subscribe" by sending $10/mo. Scale that up to national media outlets and the impact of advertising on our day-to-days
could be significantly reduced if with distribute the cost of creation among the audience willing to support those
creators.

It is my belief that given the correct platform where users can subscribe to feeds, read content, and interact with each
other, this could be the new norm. Stay tuned for when I release that platform at some point before the heat death of
the universe (hopefully).

[^1]: [RSS 2.0 Specification](https://www.rssboard.org/rss-specification)
[^2]: If you're using .NET, you can use [my library](https://www.nuget.org/packages/PodcastIndexSharp) to make this even
easier. Other libraries and examples are available on the
[Podcast Index Docs](https://podcastindex-org.github.io/docs-api/#overview--libraries)
[^3]: [podcast-namespace](https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md)
[^4]: [Podcast 2.0 Apps](https://podcastindex.org/apps)