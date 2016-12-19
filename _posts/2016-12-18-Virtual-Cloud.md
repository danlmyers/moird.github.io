---
layout: post
title: Virtual Cloud
comments: true
---
![http://imgs.xkcd.com/comics/the_cloud.png](http://imgs.xkcd.com/comics/the_cloud.png)

Companies and people have put a lot of faith into the cloud.  The cloud though has been fairly loosely defined in the early days of its existence.
However at this point I think it is fairly clear in the sense that it is resources, compute/networking/storage that are someone else's responsibility
to maintain.  A significant amount of companies in general are utilizing different clouds.  Which makes sense, they can easily stand up virtual
machines, or containers without having to pay to manage all the equipment themselves, just pay someone else to manage it all.  Which is probably
fairly reasonable in the grand scheme of things.  Managing datacenters, equipment, servers, network gear, SANs can be expensive and require
people with a wide array of skills to properly manage.  Or pay a little overhead to run in the cloud.  It also doesn't seem like clouds that
are being shutdown mean much.  Cisco is shutting down their public cloud and there are some news articles about it, but the stock market
seemed to shrug it off as the share price of Cisco hardly changed at all with the news [Cisco discontinues cloud](http://www.geekwire.com/2016/another-one-bites-dust-cisco-discontinues-1b-cloud-initiative-aws-azure-others-expand/). 
Of course with the news of cisco is that other clouds are expanding, but it is the same couple of clouds that are proving to be the most 
successful in this realm.

At the end of the day though everyone is relying on these few companies to provide the majority of their services and not really looking at the
general risk of running everything in the cloud.  On the other hand there is risk of running everything locally and on premises.  At this point
I think that it is very situational.  Taking this one size fits all of shoving everything into the cloud is a bad idea.  It is kind of gotten
to the point of being too easy so put it in the cloud.  I also think that running everything on premise is not quite the way.  Probably be
best served by having things setup in a variety of different clouds and on premise solutions and move machines easily between them.
Containers work well in that particular space with that idea, being easy to have up and running wherever they are needed and easy to start
up.  Their real downside though is persistent data and some services probably are not the best in a container.  There are other services
that are not really well suited to run blindly in the cloud either or in every cloud.  For instance [mail in a box](https://mailinabox.email/guide.html)
mentions that running on aws probably will not work that well since aws is blocked by a lot of providers due to spam.  The majority of the
solutions at this point revolve around running everything in the cloud, multiple clouds, hybrid, or on premises.  Wonder if the general consensus
at this point would be to just run in multiple clouds?