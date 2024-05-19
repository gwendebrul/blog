# Offline Webpages

## Preface

So a couple of weeks ago my **internet** went down, it was something on the **provider's** end. And when the **internet** goes down there's no more coding that I can do. I'm the kind of developer who's always looking up stuff, and for lloking up things I need the **internet**.

At the same time I saw a video on **youtube** from someone who host it's own **internet** pages like **wikipedia** and many more. This got me thinking, I got a **server** which I can use for it.

Let's do it!!

## Kiwix and zim files

In the video the creator talked about [Kiwix](https://kiwix.org/en/) and zim files. There are some pages I wanted to be accessible offline when the **internet** is down.

- wikipedia
- stackoverflow
- ask Ubuntu
- ask Different

These are the most important for me that are accessible with Kiwix. 

## Other resources

But that's not the end of my wish list of **offline** **web pages**. Here's a list of all the **web pages** I wanted and already have.

- geeks4geeks (maths)
- perldoc
- cpan
- metacpan
- w3schools
- msx.org

The downside is they are not accessible through kiwix.
But not all hope is lost. I remember a tool from years ago which I used briefly then, it's called [SiteSucker](https://sitesucker.en.softonic.com/mac?ex=RAMP-2046.0).

## Download sites through SiteSucker

It's very easy to download **websites** with **SiteSucker**. Just enter the URL and set the depth and go. The depth is important as it follows links and scrape more content. I usually set it on 4 and this give me the best result.

It can take a while to download a 'complete' **website**. I quote 'complete' because you never know when you downloaded the full **website**. As I said earlier I use 4 as depth because this download everything I need without other stuff on other **websites** which were linked on the **website** you downloaded.

## MSX.org

This website gives me a though time, because the way it's coded. Firts I needed to replace some URLs from **https://msx.org** and **https://www.msx.org** to something that's local on my server like **http://my-server-ip:8888**.

For this conversion I wrote a little **Perl script** called [replaceURL](https://github.com/gwendebrul/replaceURL). This does the job and now the links mostly points to my local **server**. Almost but not all, again this is the way the **website** is coded. For example photos are not correctly linked to. No matter what I try, the images doesn't show.

## Other websites

All the other website works great, only the search field doesn't work. Which I expected, because this queries a database which is not present on the local **server**. You only download the **HTML** files.

But **websites** like **Perldoc** can be used without the search functionality. On some **websites** you have to dive into the links yourself from the menu's, but that's ok, at least it works.

## What's next

Next is to install [Kiwix](https://kiwix.org/en/) and download **stack overflow** for example and try if the search works or not. If not than I won't be getting any further into Kiwix. If the search works than I will download the other **websites** that are on my list.

For now the most important **websites** for my **programming** are available on my local **server**. So when the **internet** goes down again, there are no excuses for not keep **programming** ;-)