---
layout: post
title: "Decoy Routing"
date: 2017-02-10
---

-Domain-Censor Collusion in Decoy Routing

I was just catching up on some academic reading by looking over CCS 2016 and found this paper, Game of Decoys: Optimal Decoy Routing Through Game Theory. It sparked an idea, that others have probably had, about what seems like an unaddressed possibility in the hypothetical world where Decoy Routing systems actually get implemented: does the censor have leverage to get domains to move to non-decoy routing Autonomous Systems (ASs)?

Since that question might not make any sense, I've tried to provide a quick summary of what Decoy Routing is below. Otherwise, feel free to skip down to Domain-Censor Collusion
1 What is Decoy Routing?

Decoy routing is a censorship circumvention technique that underlies a handful of published censorship circumvention systems (Cirripede, Curveball, Slitheen, Telex, TapDance). At a high-level, the idea is to get out of a censored region by making an HTTPS request to a non-censored pseudo-destination. Then, hide a special signal in the connection that activates a cooperating router along the way. The router will be outside the censored region, but otherwise could be anywhere between the user and the pseudo-destination. This signal will tell the router that, actually, the user wants to connect to some other, censored, destination. The router then obliges by fetching the censored content and sending it back to the user instead of (or in addition to, or in some more complex way mixed in with) the content of the pseudo-destination.
2 Routing Around Decoys (RAD)

There are a few attacks possible against Decoy Routing systems. The censor could break all HTTPS connections and view the decrypted traffic, but most assume this is not going to be done because of various costs. The censor could also do different types of traffic fingerprinting to detect the difference in content from the expected content of the pseudo-destination, but again development of new systems appear to be successful at preventing this. At least, the fingerprinting attacks are not fundamentally a problem for Decoy Routing.

The fundamental weakness in Decoy Routing is the attack called Routing Around Decoys (RAD). This is as simple as it sounds: the system depends on the user's packets passing by a cooperating router; the censor can probably know which routers are cooperating, so it can cause traffic to be routed so that those cooperating routers are avoided. However, there has been a lot of debate, and multiple papers, arguing whether or not this attack is feasible in practice. So now we are caught up on the background.
3 Domain-Censor Collusion

The costs to a censor to routing around decoys (RAD) are several, according to Nasr and Houmansadr: quality of service suffers from longer routes, some domains may be unreachable, some routes may now require payment to get traffic through because of peering agreements (or lack thereof). Basically, RAD has costs because the domains a censor's people want to get to are either inside or optimally-through a decoy routing AS. What might change that, besides an AS not participating in decoy routing? The domain could expand/move itself to a different AS!
4 Why Would Domains Collude?

The utility function in Nasr and Houmansadr's game is only for the censor; but the domains whose connections get worse beause of decoy routing could suffer as well. If I run a domain and I am losing customers because their traffic route has poor service, or because I am hosting my content in a censored AS, I am losing business. Assuming most businesses do not care one way or the other about censorship, they would have no problem expanding hosting to a different AS, just as they might add webservers in new locations to provide better service regardless of censorship. In the case of larger censors, like China, many technology companies have already made compromises to their procedures or standards in order to get access to the Chinese market. How many businesses would turn down access to the Chinese market just because they have to host extra webservers in a Chinese-friendly network? I am guessing not many, at least not many that think they could get much Chinese business in the first place.

I think the idea that non-censored domains might actively - if indirectly - help a censor keep RAD economical is an interesting possible twist to a decoy routing deployment. Obviously the potential effect varies significantly depending on how big a customer-base the censor represents, and how much of a burden it is for domains to add hosting in a censor-friendly AS.
