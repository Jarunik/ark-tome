# Ark Blockchain News

I started the “Ark Blockchain News” as a small prototype and added more and more
features over time. I think it is time to explain it in a bit more detail. It
does not look like much but it is pretty cool for it’s simplicity. Here is a
short impression of how it looks like right now.

![1](/img/ark-blockchain-news/1_1krow2yqzenj1YZWYsrcGQ.png)

As you can see it is a news feed that can be published on any web page. But
there is a bit more to it.

### What is the underlying concept?

The news are based on the Smartbridge feature and the Ark blockchain itself.
When you send a transaction to a specific address the information in the
Smartbridge field will be stored in the blockchain. As the blockchain is just a
distributed database you can fetch this information again and display it on a
web page.

![2](/img/ark-blockchain-news/1_O-gU-DvgiZGnR_EMVf987A.png)

The above example shows a news transaction in the blockchain explorer. This
means you can also read the news in any Ark blockchain explorer. It is not very
comfortable but it works:
[https://explorer.arkcoin.net/address/AZHXnQAYajd3XkxwwiL6jnLjtDHjtAATtR](https://explorer.arkcoin.net/address/AZHXnQAYajd3XkxwwiL6jnLjtDHjtAATtR)

### How can you post some news?

It is pretty easy. You just need to install the Ark Desktop Client and get some
Ark. If you don’t have it yet you can get the Ark Desktop client here:

<https://github.com/ArkEcosystem/ark-desktop/releases>

If you want to buy some Ark you can do so on any Exchange. Here is an example:
[https://bittrex.com/Market/Index?MarketName=btc-ARK](https://bittrex.com/Market/Index?MarketName=btc-ARK)

Now that you got your Ark in your wallet you can simply send 0.1 Ark to
[AZHXnQAYajd3XkxwwiL6jnLjtDHjtAATtR](https://explorer.arkcoin.net/address/AZHXnQAYajd3XkxwwiL6jnLjtDHjtAATtR)
including your news in the Smartbridge field.

There are currently three different type of news:

* Spam costs anything below 0.1 Ark
* Regular news cost between 0.1 and 1.0 Ark
* Premium news cost 1.0 Ark or more

So let’s post some regular news which costs 0.1 plus 0.1 Ark in transaction
fees.

#### Open your wallet

![3](/img/ark-blockchain-news/1_7Ax90-EHP1Jd7_e_g9Ge3g.png)

#### Open a transaction

![4](/img/ark-blockchain-news/1_jBkiWbGXqLAEb5Ol4BSjLw.png)

Someone stole my Ark of the last guide (as expected).

#### Send a news transaction

![5](/img/ark-blockchain-news/1_IXMXKugEGoyDbIjIOCP7SA.png)

Send a transaction of 0.1 Ark to
[AZHXnQAYajd3XkxwwiL6jnLjtDHjtAATtR](https://explorer.arkcoin.net/address/AZHXnQAYajd3XkxwwiL6jnLjtDHjtAATtR).

The important thing is to **fill out the Smartbridge (Optional) field** with
your news text.

#### Check the Smartbridge in your wallet

![6](/img/ark-blockchain-news/1_8KQaLz5DaGyEWezUHYIN6g.png)

#### Check the news on the website

![7](/img/ark-blockchain-news/1_Gzl-jd6WMOjD1nfF_4yicw.png)

Live Viewer: [https://arkcoin.net/news](https://arkcoin.net/news)

That is already it. Pretty simple isn’t it.

### What are the advantages of such news?

There are various advantage of this approach:

* The information is immutable.
* No one can censor the information.
* Everyone can build their own viewer for the news.
* Everyone can post any news.
* You don’t need a database to store your information.

### What are the disadvantages of such news?

There are of course also some limitations:

* Currently you can only post 64 characters.
* You have to pay the transaction fee (which is a bit too high currently).
* You can’t prevent malicious content.

### What enhancements might be interesting?

I got a lot of ideas already to build it out in small steps:

* Ark fee reduction will make it more usable.
* Improve different payment levels and allow cheaper news.
* Provide different news channels.
* Provide an easy to integrate script for websites.
* Write other viewers
* Use future bigger text fields of Ark for more extensive news.
* Potentially include formatting.
* An own editor to write your news.

As you can see there are a lot of possibilities. I am sure I will even find more
interesting ideas as I progress.

### Other web pages

I am happy that we already got the news feed displayed on another page.

![8](/img/ark-blockchain-news/1_KgYIYyWFniexYudxRsjzsw.png)

Chris included the news in his delegate page:
[http://www.arkdelegate.com/](http://www.arkdelegate.com/)<br> Thank you for the
support of the project!

### My project

So far it is only a simple React Component:
[https://github.com/Jarunik/arkcoin/blob/master/src/AppNews.js](https://github.com/Jarunik/arkcoin/blob/master/src/AppNews.js)

Feel free to include it in your website or build something on your own.<br> I am
also happy for any contributions or ideas.

I will sure try to keep the news flowing. Feel free to post something too!

Delegate jarunik<br> [https://arkcoin.net](https://arkcoin.net/)