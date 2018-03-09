+++
title = "Mechanism Design and Sponsored Search Auctions"
date = 2018-02-27T12:40:58-05:00
draft = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = ""
preview = true

+++


If you are watching a video on You Tube and the video is paused for an annoying 30 sec advertisement, you probably should know the history behind on-line advertisements. In fact, advertising and internet helped each other attain their respective potentials. Without advertising, there wouldn't be any Google or Facebook (may be not in the present form), and without any internet, you probably wouldn't know about a tourist package in a remote island like Madeira.


In this blog post, let us have a look at a mechanism design called [_Sponsored Search Auctions_](https://en.wikipedia.org/wiki/Sponsored_search_auction), which is key to the rise of internet giants like Yahoo! and Google. We shall discuss the properties of such auctions along with examples to gain a better insight. This post is intended for general audience, and hence we shall try hard to not drift into game theoretic concepts (although it is impossible to do so at all times).

Sponsored Search Auctions
======

Before diving into the topic, let us get familiar with auctions. [_Auction_](https://en.wikipedia.org/wiki/Auction) is the sale of items where the seller invites the buyers (bidders) to submit bids and makes a decision on whom to sell it to.  The bidders have their own valuations for the item and submit bids indicating their willingness to pay for it, and the seller allocates the item to the bidder he/she likes the most (usually it is the bidder with the highest bid).  For example, if you want to move to a new place but you do not want to carry some of your furniture/luggage, you put them on
eBay for an auction. If someone fails to repay their loan to a bank, the bank
auctions off the mortgaged property.

Consider that you are planning to travel to Madeira with your friends. A quick internet search for the work 'Madeira tours' results in a list of links some of which
are marked as _Ad_ sponsored by the respective websites. Whenever a user clicks
a sponsored link through the search engine, the advertiser pays a certain amount
to the search engine for facilitating their advertising. In _Sponsored Search Auctions_ for every keyword, the search engine allocates a certain number of slots, and multiple advertisers compete for these slots which provide different value to different advertisers. The probability of an advertiser $i$’s Ad being clicked when placed in a slot $j$ is called the **Click Through Rate**, or CTR, denoted by $α\_{ij}$ (this is simplified in various mechanisms). The per click value to advertiser $i$ is $s\_{i}$ and the expected value on being assigned a slot $j$ is $α\_{ij}s\_i$.  The search engine must design an efficient mechanism that maximizes the welfare (or the revenue) by assigning the slots to different advertisers (bidders). Let us follow the convention of using search engine for the seller and advertiser for the bidder.

Let us now look at some important properties of _Sponsored Search Auctions_ that differentiate it from traditional auctions.
- These auctions are dynamic in nature and the bidders can change their bids from
time to time. We may now not get the same list of links for searching 'Madeira tours' if some bidders' algorithms modified their respective bids.
- The items at sale are perishable with time. If an item is unsold, it is of no value to the search engine at a later point of time, unlike a storable product such as a piece of furniture which still has some value in the future if unsold. The search engine though is not constrained by the quantity of items it can offer. It can potentially fill up the entire search results with _Ads_. However, we may not use _Google Search_ if every search query returns only _Ads_.
- The unit of what is being sold is different for the search engine and the advertiser. For the search engine, it is the space allotted to the advertiser every time a user searches for that particular key word. For the advertiser, it is the value of trade when a user clicks the sponsored link. In practice, the advertiser is charged based on the number of clicks (or the rate of it) the link receives.

History and Generalized First Price Auctions
============================================

In the early 1990s, when the era of internet advertising began, _Ads_ were sold on a per impression basis, which includes a flat fee paid by the advertiser to the search engine for showing their advertisement a fixed number of times. Internet advertising was then revolutionized by Overture in 1997 (later acquired by Yahoo!), with their design called as the [_Generalized First Price Auction_](https://en.wikipedia.org/wiki/Generalized_first-price_auction). It is similar to a traditional open bid first price auction. The important features of this auction are as follows
- Advertisers can now target their _Ads_ for specific keywords instead of tickers or banners
- Advertisers can bid their willingness to pay on a per-click basis, instead of a flat price. The payments were also immediate and paid per click
- The bids were arranged in decreasing order, with the highest bid getting the slot with the highest CTR (usually the top most slot), the second highest bid getting the second best slot and so on.
- If an advertiser wins a slot, the advertiser has to pay value equal to their bid times the CTR of the slot.
- Advertisers also were allowed to change their bids frequently by looking at the other bids

Owing to its novelty and advantages over the previous mechanisms, Overture's GFP was a success and Yahoo! and MSN were among the top users of GFP. However, the mechanism itself was unstable and flawed.

### **Example**:
Consider that for a specific key word, there are three slots and four advertisers competing for the slots. The click through rates of the slots are 400, 200 and 100 (clicks per hour). The advertisers 1, 2, 3, 4 have values $ \$10, \$5, \$2, \$1$ respectively.Note that the advertisers know the values of the other bidders.

Under the GFP mechanism, suppose advertiser 3 bids $ \$1.01$ to guarantee that he gets a slot (because advertiser 4 would not want to pay more than her value, i.e.  $\$1$). Noticing this, advertiser 2 would revise her bid to $ \$1.02$ because she does not need to pay more than that to get the second best slot. Similarly, advertiser 1 would want to revise the bid to $ \$1.03$ for the same reason. But advertiser can now revise the bid to $ \$1.04$ to win the top slot (CTR 400) which is four times more valuable than the third slot (CTR 100). And, the cycle continues.

Clearly, there is no pure strategy equilibrium, and the competitive revision of bids continues. This gives an unfair advantage to bidders with high compute power to algorithmically outbid their competitors. Remember that the goal of the search engine is to maximize it's own profit, the amount earned through the payments made by the bidders.


Generalized Second Price Auctions
======
Realizing that no bidder at position $i$ (in the decreasing order of bids) wants to pay more than the bidder at position $i+1$, as can be seen from the above example,
Google came up with their own pay-per-click system, called AdWords Select in February 2002. Their system follows a [_Generalized Second Price_](https://en.wikipedia.org/wiki/Generalized_second-price_auction) mechanism, where the advertiser at position $i$ pays the bid of the advertiser in position in $i+1$ plus a minimum increment (say $ \$0.01$). Let us consider the above example and evaluate payments made while employing a _GSP_ structure.

### **Example**:
Consider that the advertisers bid truthfully (i.e. bid their actual values). Then the three slots 1,2,3 go to advertisers 1,2,3 respectively.
- The payment made by advertiser 3 is $ \$101\ (=1.01\times 100)$ and the profit is $ \$99\ (=2\times100-101)$
- The payment made by advertiser 2 is $ \$402\ (=2.01\times200)$ and the profit is $ \$598\ (=5\times200-402)$
- The payment made by advertiser 1 is $ \$2004\ (=5.01\times400)$ and the profit is $ \$1996\ (=10\times400-2004)$

The total payment received by the search engine is $ \$2507$. Contrastingly, in the _GFP_ example, there was no equilibrium payment like above.

Let us consider another example when the advertisers do not bid truthfully.

### **Example**:
Consider that the CTRs of the three slots are comparable, say 400, 390, 380 respectively. Then if everyone bids truthfully, advertiser 1 pays $ \$2004$ and the profit is $ \$1996$. However, if advertiser underbids, say $ \$2.02$, he can still ensure slot 2. His payment is then $ \$3112=(10-2.02)\times390$ which is greater than $ \$1996$ if he bids truthfully.

Despite the fact that _GSP_ doesn't ensure truthful behavior, it is widely used because it is amazingly simple. It is interesting to note that most search engines including Google still use a variant of _GSP_.



Conclusion
==========
Around two decades ago, innovative mechanism designs fueled the growth of on-line advertising and helped the dot com industry flourish. Today, the world is set to witness revolutionary changes in the way people move from one place to another. With autonomous ride sharing, ride hailing services becoming increasingly popular, we can expect new modes of advertising (may be call it in-transit advertising) with immersive multimedia technologies and what not. Recently
[Intel and Warner Bros](http://fortune.com/2017/11/29/intel-partners-warner-bros-self-driving-cars/)  joined hands to develop tech to enhance immersive experience of riders for entertainment and advertising. I would not be surprised to expect a future where you subscribe to Ad-free rides by paying more.

It is important to develop the tech to make autonomous vehicles safe for the public, but it is even more important to build revenue models that enable ride hailing services generate revenue to stay competitive. We can only wait and see how advertising (and mechanism design) can play its part in shaping the future of our transportation.

References
------
1. Edelman, B., Ostrovsky, M. and Schwarz, M., 2007. Internet advertising and the generalized second-price auction: Selling billions of dollars worth of keywords. American economic review, 97(1), pp.242-259.
