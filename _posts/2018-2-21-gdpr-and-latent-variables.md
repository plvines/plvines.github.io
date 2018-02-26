---
layout: post
title: "The GDPR and Sensitive Data Inference"
long_description: "The GDPR outright bans the processing of several types of sensitive personal data except under special circumstances. What happens if a company reveals data that can be used to _infer_ these sensitive pieces of data? Is that also prohibited?"
date: 2018-02-21
---
_Warning:_ I am not a lawyer, and certainly not a lawyer specializing in EU privacy regulations. I just read technology law and policy as a hobby.

# The GDPR Prohibits Processing Sensitive Data
Article 9.1 of the [GDPR](http://data.consilium.europa.eu/doc/document/ST-5419-2016-INIT/en/pdf) states:

> "Processing of personal data revealing racial or ethnic origin, political opinions, religious or philosophical beliefs, or trade union membership, and the processing of genetic data, biometric data for the purpose of uniquely identifying a natural person, data concerning health or data concerning a natural person's sex life or sexual orientation shall be prohibited."

There are, of course, 11 exceptions to this prohibition. This should not be surprising, since there are entities society clearly wants able to store and process biometric and health data (e.g., secure facilities and hospitals). However, to my reading, there are only two of these that could be potential avenues for an advertising business to get around the prohibition. I do not think either of these will ultimately work, but they seem worth exploring:

## Prohibition Exception 1.
GDPR Article 9.2.a states the prohibition does not apply if:

> "The data subject has given explicit consent to the processing of those personal data for one or more specified purposes, except where Union or Member State law provide that the prohibition referred to in paragraph 1 may not be lifted by the data subject;"

I presume that this explicit consent for sensitive personal data would need to be able to be made separately from consent to non-sensitive data gathering. So in order to use this exception, an advertising business would need to specifically ask a user something like:

"Do you agree to allow your gender to be used to select advertisements to show you."

If this consent can be obtained more subtly or bundled with agreement to use the service then perhaps this is a reasonable path. However, that would seem to trivially undermine the a major purpose of the GDPR, so I assume EU courts will demand something more substantial.

## Prohibition Exception 2.
GDPR Article 9.2.a states the prohibition does not apply if:

> "Processing relates to personal data which are manifestly made public by the data subject;"

This clause is unclear to me, because I do not know the definition for data to be "manifestly made public." In the context of the Internet, is setting a Facebook profile to "interested in: women" and allowing it to be seen publicly the same as making one's sexual orientation manifestly public?
We will revisit this idea of sensitive data being manifestly public later in this article when combined with the idea of latent variables.

# Latent Variables: Turning 'Boring Data' Into Sensitive Data
[Latent variables](https://en.wikipedia.org/wiki/Latent_variable) are a very simple idea: we want to accurately _infer_ a piece of unobservable data from one or more pieces of observable data?

An in-context example: suppose we wanted to know the sexual orientation of Bob, but he did not helpfully provide it as public Facebook profile information. So sexual orientation is _unobservable_.

However, suppose we do know what apps he has installed on his smartphone. Most of these probably have nothing to do with his sexual orientation, but perhaps he has one or more dating apps tailored to specific sexual orientations (e.g., Grindr and JackD are designed for the homosexual dating community). In that case, we could use the _observable_ variables of having Grindr and JackD installed on Bob's smartphone and _infer_ the _unobservable_ variable that Bob is gay.

Obviously we might be wrong; Bob could have those apps installed for any number of reasons. However, given enough data, we could get an estimate of how accurate this inference is. If 99.999% of people with Grindr installed are gay, then we can probably confidently assert that Bob is gay based on having Grindr installed.

Inference like this is not absolute truth, but with a reasonable accuracy it is close enough to truth for advertisers. As far as [we know](https://www.propublica.org/article/facebook-advertising-discrimination-housing-race-sex-national-origin), this is indeed how ad targeting features like Facebook's ethnic affinities work; Facebook does not have an ethnicity box for a user to fill out, but they use other data they _do_ have (whether from Facebook itself or from other data sources they buy) to infer that user's ethnicity.

# GDPR: Probably Allowed?
With the above pieces in place, the question is: how does the GDPR interact with latent variables that it considers sensitive data? This question really has a number of more specific aspects.

## If Data is Inferred is It Not Sensitive?
This goes to an example like Facebook's ethnicity ad targeting. One could take the position that the GDPR prohibition against sensitive data only applies to data that is true - since inferred data like Facebook's ethnic targeting is not guaranteed to be correct, maybe it is not sensitive. This seems like a bad argument, since any data could be flawed in some way such that we cannot be 100% sure a given piece of sensitive data is absolutely true, but the GDPR prohibits its gathering and processing anyway.

The conclusion, then, is that Facebook will need to stop creating and providing this ethnicity ad targeting feature for EU residents, unless it can find itself in one of the excepted clauses. Not to single-out Facebook, many other advertising businesses of various types will need to stop collecting or inferring various types of sensitive data covered by the GDPR. In fact, Facebook probably has a decent shot at the explicit consent option, particularly for other sensitive types of data that _are_ directly provided by users, like sexual orientation or political affiliations. Whether or not the GDPR still allows those to be used as ad targeting features seems less certain to me.

## If Data is Inferred but Benign Sounding is It Not Sensitive?
Many ad targeting options are very easy to map to the GDPR's prohibited data wording: e.g., Facebook labels its ethnicity targeting as literally ethnicity targeting. However, what if a platform inferred the same data, but named it something less sensitive? After all, this data was never entered by a user who needed to understand what they were entering. So there was never a need for a users to see a "select ethnicity" box and understand what to put there. Instead, Facebook's algorithms could simply infer the same data and label it "Marketing Category 5" if they wanted. Is this data not sensitive, just because its title is different? If so, at what point does it become sensitive again? If marketers were instructed that it correlated highly with ethnicity, would it then be sensitive?

## Is a Proxy of Sensitive Data Also Sensitive?
Following from the point above, if there is a certain type of data that is highly correlated with sensitive data, does that data _become_ sensitive? An easy example could be language: almost every ad targeting service I have seen allows targeting based on language, for obvious reasons since showing ads in a language your audience does not understand is not very effective. However, language could also be an easy proxy for specific ethnic populations. Similarly, targeting certain interests or past purchases could be an extremely accurate proxy for gender.

Thus, even without access to large amounts of raw data and fancy algorithms, a marketer using an ad platform that has dutifully removed all GDPR-offending data, could probably still easily target ads based on ethnicity, gender, or a number of other prohibited factors, solely based on a few proxy variables that _are_ provided as targeting criteria. Would the GDPR have anything to say about this? My guess is no, this would be perfectly acceptable.

I have two pieces of evidence for this opinion: First, a simple intuition that it would be impossible to regulate preventing data that could be used to infer sensitive data. Second, clause 51 states:

> "The processing of photographs should not systematically be considered to be processing of special categories of personal data as they are covered by the definition of biometric data only when processed through a specific technical means allowing the unique identification or authentication of a natural person"

So, photographs _can_ be used to generate sensitive data (biometric, in this case), photographs are _not_ considered sensitive in themselves unless one actually performs this generation. Now, I suppose the existence of this specific clause could read inversely, that this specific case is an exception and generally data that can be processed into sensitive data is automatically sensitive.

If it _is_ the case that any data which can be processed to generate sensitive data is "tainted" and considered sensitive as well, then I imagine the GDPR will be an even wilder ride than already thought. Current research shows all kinds of correlations between benign and sensitive data exist, and it seems likely this list will keep growing. 

# Conclusion
To summarize my thoughts: the GDPR fairly clearly prohibits providing ad targeting features like "ethnicity", "sexual orientation", or "political affiliation", whether that data was directly gathered or generated by processing other, non-sensitive, data. So, to give a specific example, Facebook's ethnicity group, sexual orientation, and political or religious targeting criteria should probably disappear when targeting ads in the EU. However, Facebook and other social networks might manager to sidestep that by claiming user consent, other ad networks that are not also social media platforms would probably not be in a position to get similar consent.

Inferring sensitive data but calling it by another name is probably okay. It is not clear how you would go about proving the data is "really" sensitive data masquerading under another name. Particularly because it could really be nonsensitive data that just so happens to correlate highly with sensitive data.

Lastly, related to the second point, data can be used to derive sensitive data, even if doing so is trivial, will not be prohibited. Trying to rule otherwise and regulate _potentially_ sensitive data would be a very slippery slope, subject to continuous revision due to research studies and as such seems completely untenable.

