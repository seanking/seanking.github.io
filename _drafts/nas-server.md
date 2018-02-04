# NAS Server

For years I used a [2009 Mac Mini Server](https://arstechnica.com/gadgets/2010/01/mac-mini-with-snow-leopard-server-review/) for network storage, Time Machine backups, GitLab Server, and Build Server. The Mac Mini worked great throughout the years, but it is 2018 and time to upgrade. The only problem is that Apple is depreciating features in the next release of macOS Server and has not provided a substantial hardware upgraded to the Mac Mini in years.

I knew that I didnt't want to build my own NAS server. I did that for years before my Mac Mini and it was always more maintance than I wanted to endure. I decided to buy a product and have someone else support it.

Like most tech products that I am interested in, I went to the [Wirecutter](https://thewirecutter.com) to read their reviews on [The Best NAS for Most Home Users](https://thewirecutter.com/reviews/best-network-attached-storage/). Not to my surprise, the NAS server market was still dominated by QNap and Synology.

Both the [Qnap TS-251A](https://www.qnap.com/en-us/product/ts-251a) and [Synology DS218+](https://www.synology.com/en-us/products/DS218+) scored the highest marks. Each had similar features that were important to me.

* Web Interface with HTTPS
* RAID 1 mirror
* Hot Swappable Drives
* Encryption
* Docker (GitLab, Nexus, etc...)
* UPS Support
* 8 GBs of RAM
* 2 Hard Drive Bays

In my review the NAS Servers the biggest win for Synology was that it supported [btrfs](https://en.wikipedia.org/wiki/Btrfs), were QNap used [ext4](https://en.wikipedia.org/wiki/Ext4). Since this was going to be a file server that was mainly for storing backups and version control, btrfs made the Synlogy DS218+ a clear winner for me.

Btrfs is a CoW (copy on write) that filesytem supports checksums and snapshots. Anyone that has read about [bit rot](https://en.wikipedia.org/wiki/Data_degradation) should be scared of storing any important data on hard drives that don't support checksums and self-healing. Btrfs data protection and recovery mechanisms prevent bit rot from occuring on your important data (Photos). Synology provides a good overview of the important features of [btrfs](https://www.synology.com/en-us/dsm/Btrfs).
