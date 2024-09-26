# Mac Mini 2014 Upgrade

## Start

![mac mini late 2014 info](images/mac-mini-info.png "mac mini info")

Years ago, I bought a **Mac Mini late 2014** second-hand to use for **scanning** and converting **DVDs** and **Blu-Rays** to digital files for **streaming**.

All this time, I had an **external** **mSATA** **drive** connected, which served as the boot disk with the **macOS** **operating system**.

A few months ago, I came across a **YouTube** video showing that you could install an **NVMe** **drive** internally while keeping the current **hard drive**. No more need for the **external** **drive**.

So, I decided to go ahead and ordered the necessary parts.

## Parts

![mac mini 2014 upgrade parts](images/onderdelen.jpg "mac mini 2014 upgrade parts")

First, I needed an adapter that could connect to the built-in **PCIe** port in the **Mac Mini**. These are easy to find on both **eBay** and **Amazon**.

I ordered mine from [amazon.com.be](https://www.amazon.com.be/dp/B093DJYG9G?ref=ppx_yo2ov_dt_b_fed_asin_title&th=1).

The second part was the **NVMe** **drive** itself, which I also bought on [amazon.com.be](https://www.amazon.com.be/dp/B0C2WGL8DQ?ref=ppx_yo2ov_dt_b_fed_asin_title).  
It's a **Crucial 1TB NVMe drive**.

With the parts in hand, I started the upgrade installation.

## Hardware Installation

On [iFixit](https://nl.ifixit.com/Guide/Mac+mini+eind+2014+Vervanging+van+de+SSD/32646), I found the guide to replace the **Fusion drive SSD**, but I didn’t have anything in that spot. My **Mac Mini** only had a **1TB hard drive**. However, the steps for opening the **Mac Mini** were the same, so with this guide in hand (actually on the computer ;-)), I began the installation process.

The installation went smoothly, though I had to reopen the **Mac Mini** because the **NVMe** drive wasn’t properly connected the first time.

With the hardware installation successfully completed, I thought I could now install **macOS**.

## macOS Installation

When I booted from a **bootable** **USB stick** with **macOS Monterey**, the **NVMe drive** didn’t show up in **Disk Utility**, so I couldn’t install **macOS** on it.

What do you do when something doesn’t work? Just use Google. I quickly found an article describing the same problem I had, and it turned out that you first needed to have **macOS Monterey** installed on the built-in **hard drive** for the **NVMe drive** to become available (something related to firmware).

So the next step was to install **macOS Monterey** on the built-in **hard drive**. That went reasonably quickly, about 45 minutes, I think. The **USB stick** was **USB 2.0**, so it was slower, and the installation was done on a regular **hard drive**.

Once that was done, I could finally install **macOS Monterey** on the **NVMe drive**. This time, the **NVMe drive** was recognized. After another 30 minutes, the installation was complete.

![mac mini late 2014 disk info](images/mac-mini-disk-info.png "mac mini late 2014 disk info")

Finally, I erased the original built-in **hard drive** so that the **macOS Monterey** installation was only on the **NVMe drive**.

## Usage

The **Mac Mini** is used **headless** (without a monitor) primarily for my **Canon Lide 110 scanner** and **makeMKV** to rip **DVDs** and **Blu-Rays**. I can also share the screen so that I have access to the **Mac Mini's** 'display' from my **MacBook**.

Additionally, I use it for testing another **IDE** for my **Perl** scripts, specifically **VIM**. I am currently taking a **web developer** course where we use **Visual Studio Code** with three very handy features: **format document**, **Emmet**, and **LiveServer**.

I want to have these features available in **VIM** as well. This requires a lot of testing, and I’d rather not do that on my daily **computer**.

## Costs

- The **NVMe drive** cost €66.99.
- The PCIe adapter was €12.99.

So, the total cost was €79.98.

This was definitely worth it—finally free from the external **mSATA drive** and now running a fairly recent **operating system**, **macOS Monterey**.