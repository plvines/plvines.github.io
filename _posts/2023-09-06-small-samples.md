---
layout: post
title: "Bring It On Home: An Argument for Using Small Samples to Model Patterns of Life"
long_description: "Some thoughts on why research on creating synthetic user behavior is tractable even without large data sets."

date: 2023-09-06
---

Covert network channels are continually increasing in sophistication. Most of these advances have centered on defeating per-packet or per-connection detection. These are reasonably seen as the highest priority attacks to mitigate, since they are actually seen in-the-wild with national censors and enterprise firewalls. However, an analysis of the system’s behavior over time and across multiple connections – its _pattern of life_ – can also provide a method for detecting its use. This is particularly true for channels that explicitly appear to be an allowed application like email or videochat. When the covert usage does not match the observable application, such as using email to tunnel Twitter ([Li et al 2016](https://censorbib.nymity.ch/pdf/Li2016a.pdf)), detecting usage of the covert channel can be easy.

The 2022 PETS paper [Learning to Behave: Improving Covert Channel Security with Behavior-Based Designs](https://petsymposium.org/2022/files/papers/issue3/popets-2022-0068.pdf) explicitly addresses this case, and develops the concepts of _behavioral independence_ and _behavioral realism_ to express the properties covert channels need. The former means the behavior of the covert channel must be independent of the covert application: e.g. a single email will be sent at a certain time, regardless of whether the application has any data to send or whether it has more data to send than can fit in a single email. The latter means this independent behavior must be realistic and thus difficult to distinguish from a normal user of the observable application: e.g. email usage should look like normal user behavior (and thus generate many false positives if the adversary tries to detect it).

A common challenge to building solutions in this space is defining what “normal user behavior” actually is. In some sense it’s impossible to elegantly “solve” this challenge: inherently user behaviors vary from user-to-user and if one tried to take some kind of averaging approach the resulting average behavior might easily not represent any real user behavior. Unfortunately, a common reaction is to try to avoid the complexity by making arguments for pushing user behavior profiling out-of-scope.

One arguent is that user behavior is too varied for an adversary to possibly employ profiling at whatever scale the system aims to operate at. This still requires some type of _variance_ in system behavior to avoid a static fingerprint that _is_ tractable for the adversary to filter for. 

Another argument is that whatever profiling _is_ done, will be done at layers that conveniently align with data the builders can obtain, like total bandwidth usage rather than application or connection data. This assumes an adversary that is clearly weaker than known cases in related contexts. E.g. existing research on social bot detection shows use of clickstream data for characterizing user behavior ([Wang et al. 2013](https://www.usenix.org/system/files/conference/usenixsecurity13/sec13-paper_wang_0.pdf)).

## The Global Sample Trap

However, I suspect the real challenge usually encountered by system developers in these situations is not the theoretical difficulties of accurately modeling such varied real user behaviors. The real challenge is getting representative user data to begin with. If we do not know the baseline normal user behavior the adversary will be looking at, how can we be sure our system will blend in? It is tempting to say what is needed is some kind of broad representative sample of user behavior profiles to build and evaluate this system. For example, directly analogizing research in similar domains, like evaluating Obfs classification, requires this. But this is a trap. 


## We Just Need One (or, a few…)

We do not need our system to emulate any arbitrary real user behavior to successfully blend in; it just needs to emulate behaviors common enough to overwhelm adversary resources and varied enough to avoid a static fingerprint. The adversary has to have a representative dataset of their entire userbase and be able to model all different types of users in order to avoid false positives: the system only needs to have data to build a model of a single type of user. If not for fingerprinting, even just exactly replaying a single user profile copied from a real sample should be sufficient.

So relatively paltry data sources are potentially sufficient for building a useful model of user behavior. They are also sufficient for conducting an evaluation of that model as long as the evaluation includes a notion of anomaly detection. One implementation of this concept is a GAN methodology, just as the Raven paper uses. 

## Localization Dangers

An argument against the validity of models built on small samples is localization: if a small user sample is used and those users are not from the locale the adversary operates (whether that is a specific country, a population, a workforce, etc.) then there is a risk that the sample and the adversary’s real users are behaviorally disjoint. A very simple example would be a model that includes UTC times and is trained on data from one timezone but then deployed in a drastically different timezone: even if the per-day behaviors are similar to the real users, the wrong timezone means it would be easy for the adversary to detect the model. 

There is no definite way to avoid this issue with pre-built models: individual dimensions of localization differences can be identified and addressed (e.g. timezone-based behaviors have a straightforward fix) but, ultimately, local user behavior profiles have to either be sampled or assumptions must be made. Sampling a small number of behavior profiles from the real locale is, however, much more feasible than a globally representative sample.

The logical next level of complexity is to have some level of in situ adaptation or tuning: modify or construct the model behavior outputs based on local conditions. This approach introduces many new challenges: how to gather this local data from more than just the operator; what to do if the operator does not use the relevant applications themselves; how to conduct modifications in resource-constrained environments; how to make the entire system usable. Even more than pre-built models, dynamically updated approaches will need many application-specific nuances and tricks to form a functional system.

### Global Sampling Does Not Help

Localization can even be an argument against trying to obtain and use representative global user profile samples: if the adversary can restrict their analysis to a subset of the global users then a model built to blend into that global space may generate a majority of behaviors that fall outside the space of the adversary’s subset. A simple example of this exists in the network protocol space: tunneling a censored application through Minecraft may appear to be an excellent approach at the global scale, where there is a significant number of Minecraft connections. However, if moved to an enterprise network context it suddenly becomes easy to detect and block. Similarly, using a look-like-nothing protocol like Obfs is well-motivated against a nation-level adversary that needs to allow many proprietary and esoteric protocols for fear of accidentally blocking something like medical device traffic. However, moving to a more restricted setting enables protocol whitelisting, which invalidates look-like-nothing approaches.

The Raven paper illustrates a related point when they perform an analysis of detectability on their user behavior model under differing adversarial datasets. The most successful adversary detector is trained on exactly the same dataset as the user behavior model; when the detector training dataset is supplemented with additional user datasets not used to train the behavior model, detection performance goes down because this additional (real) data is just additional noise. So long as the user data sample is present in the localized version, the most rigorous detectability analysis will be based on using the same small user data sample and the real world adversary that must include more user data in their model will always be at a disadvantage.

## Conclusion

In summary, the difficulty of acquiring large and/or globally representative application user behavior datasets should not stop researchers from pursuing user behavior models for these types of systems: small samples are actually better in a number of ways, with the caveat that they must be validated to be represented in the intended deployment locale.

## References

- Li, Shuai, and Nicholas Hopper. “Mailet: Instant Social Networking under Censorship.” Proc. Priv. Enhancing Technol. 2016, no. 2 (2016): 175-192.

- Wails, Ryan, Andrew Stange, Eliana Troper, Aylin Caliskan, Roger Dingledine, Rob Jansen, and Micah Sherr. “Learning to Behave: Improving Covert Channel Security with Behavior-Based Designs.” Proceedings on Privacy Enhancing Technologies 3 (2022): 179-199.

- Wang, Gang, Tristan Konolige, Christo Wilson, Xiao Wang, Haitao Zheng, and Ben Y. Zhao. “You are how you click: Clickstream analysis for sybil detection.” In 22nd USENIX Security Symposium (USENIX Security 13), pp. 241-256. 2013.


(This post originally appeared on the [Two Six Technologies blog](https://twosixtech.com/blog/bring-it-on-home-an-argument-for-using-small-samples-to-model-patterns-of-life/))
