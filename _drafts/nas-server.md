---
layout: post
title: NAS Purchase
---
# NAS Purchase

I used a [2009 Mac Mini Server](https://arstechnica.com/gadgets/2010/01/mac-mini-with-snow-leopard-server-review/) for years to support my need for a NAS. The Mac Mini was used for network storage, Time Machine backups, a GitLab Server, and a Build Server. The Mac Mini worked great throughout the years, but it is 2018 and time to upgrade. Apple has forced my hand to look elsewhere because they are deprecating key features in the next release of macOS Server and they have not provided a substantial hardware upgrade to the Mac Mini in years.

I knew that I didn't want to build my own server. I have been there and done that for the years before my Mac Mini. Managing my own server was always more maintance than I wanted to endure. So for this purchase I decided to look at the NAS solutions that are on the market.

Like most tech products that I am interested in, I went to the [Wirecutter](https://thewirecutter.com) to read their reviews on [The Best NAS for Most Home Users](https://thewirecutter.com/reviews/best-network-attached-storage/). Both the [Qnap TS-251A](https://www.qnap.com/en-us/product/ts-251a) and [Synology DS218+](https://www.synology.com/en-us/products/DS218+) scored the highest marks.

The following were the most important features to me in choosing a NAS.

| Feature                 | Synology DS218+ | Qnap TS-251A
|-------------------------|-----------------|-------------
| Web Interface           | Supported       | Supported
| RAID 1 mirror           | Supported       | Supported
| Hot Swappable Drives    | Supported       | Supported
| Encryption              | Supported       | Supported
| Bonjour                 | Supported       | Supported
| Wake-On-LAN             | Supported       | Supported
| Time Machine Support    | Supported       | Supported
| Rsync                   | Supported       | Supported
| Size Quotas for Folders | Supported       | Supported
| Docker                  | Supported       | Supported
| UPS Support             | Supported       | Supported
| Two Hard Drive Bays     | Supported       | Supported
| Upgradable RAM          | Supported       | Supported
| Btrfs                   | Supported       | Unsupported

Since this was going to be a server that was mainly for storing backups and version control, the Synlogy DS218+ support for [btrfs](https://en.wikipedia.org/wiki/Btrfs) made it a clear winner for me.

Btrfs is a CoW (copy on write) filesytem that supports checksums and snapshots. Anyone that has read about [bit rot](https://en.wikipedia.org/wiki/Data_degradation) should be worried about storing important data (Photos) on hard drives that don't support checksums and self-healing. Btrfs data protection and recovery mechanisms prevent bit rot from occuring. Synology provides a good overview of the important features of [btrfs](https://www.synology.com/en-us/dsm/Btrfs).

The QNap did have some advantages over the Synology, but they weren't important to me. The QNap supports being directly plugged into a TV via HDMI and comes with a remote for media controls. I don't see myself using the NAS as a media server, but if I did, I would use [Plex](https://www.plex.tv) Media Server and stream media using the Plex clients for iOS, Andriod, Apple TV, Chromecast, and Tivo.

The one disappointment I have with my purchase has nothing to do with the Synology, but more with the hard drives that I purchased. I purchased two [3TB Western Digital Red](https://www.amazon.com/Red-3TB-Hard-Disk-Drive/dp/B008JJLW4M) drives. I chose these drives because I read they were quiet and were designed to run in a NAS. It is true that the drives are quiet...  in idle conditions. However, the drives are **loud** when accessing the platters [^1]. The drive access was so loud that I had to move the server out of the office and into another room.

Overall, I am happy with my purchase of the Synology DS218+. I am able to do all the things that I required from a NAS. Time machine backups are much quicker now than with my Mac Mini, thanks to the new hardware. The support for running Docker containers allowed me to quickly spin up a [GitLab](https://about.gitlab.com) server and a [Nexus](http://www.sonatype.org/nexus/) server. 

[^1]: It appears that the newest WD Red drives are louder than the original WD Red drives released a few years back. Most of the older reviews regarding the sound levels of the drives can be misleading.
