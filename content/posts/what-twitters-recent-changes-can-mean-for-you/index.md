---
title: "What Twitter's recent changes can mean for you?"
date: "2012-10-17"
categories: 
  - "internet"
---

Twitter recently [announced](https://dev.twitter.com/docs/api/1.1/overview) [some](https://dev.twitter.com/docs/terms/summary) [big](https://dev.twitter.com/terms/api-terms) [changes](https://dev.twitter.com/blog/changes-coming-to-twitter-api) for the developers with the announcement of their new API. First of all, you need to understand what is an API. API or Application programming interface is a set of rules which are used by the applications to communicate with the service. Here the twitter related apps you use use an API provided by Twitter to communicate with it. Twitter announced some changes in their API by releasing an updated version of it. Developers have been given tile till March 5, 2013 to implement all these changes or their apps will stop working after it.

What are these changes and should they matter to you? Yes, they will impact you in a big way. We use so many apps that interact with Twitter daily. Majority of them include those third party clients which are far far better than Twitter's own official ones. Then there are apps which interact with Twitter like [Storify](http://storify.com/), [IFFFT](https://ifttt.com/), [Instagram](http://instagram.com/), [Tumblr](https://www.tumblr.com/) etc. These changes affect all these apps in one or the other way. So yes, they will affect you as well.

Below I am listing all the important changes which have been introduced and how will they affect you.

### Rate Limit Change

The first big change is the change in rate limits. Earlier apps could make 350 requests in an hour no matter what the operation is. These requests were in the form of making a search, requesting profile information, checking timeline for tweets, accessing your mentions or direct messages. The new limit is about [15 calls every 15 minutes](https://dev.twitter.com/docs/rate-limiting/1.1). Yes. From 350 requests to 60 requests. And worst part is that its not like you can use those 60 requests in your first 15 minutes because of the limit being applied in 15 minute intervals. Only the user profile lookup, profile search, tweet search, show a tweet type requests will be allowed to make 180 requests every 15 minutes. So if you are check your timeline, mentions or dms too often, you are likely to get locked out or land up in twitter jail pretty often.

### Compulsory Authentication

Earlier some of the apps could perform certain operations like searching for tweets without performing an authentication. But with the new api, every operation will require an authentication. This means that certain apps will get locked out unless they change their methods. Though this change is actually for the better as it will help keeping the spammy applications in check.

### Dropping RSS Support

Now this may hit some of you. Twitter has dropped support for RSS, XML and Atom technologies. Some of you may have usen RSS feeds of your account to access tweets via your RSS readers. Many apps also use RSS and XML technologies to access the tweets which will get locked out.

### Limiting 3rd Party clients

Its clear that Twitter doesn't want competition. They have limited all the 3rd party clients by imposing a 100,000 token limit. Every user who authenticates with a 3rd party client using 1.1 API, will be counted as a token. This token won't be released even if the user logs out. So once an app reaches this limit, it can't add more users unless previous users deauthenticate (via Settings>apps) or it asks twitter for increasing its limit. Those clients who already have more than 100,000 tokens will be limited to the double of what they have as of now. After that, they will require the permission of Twitter.

### Tweets cannot be posted to Cloud Services

Apps cannot access tweets and post them to cloud services. This has already started showing impacts. IFFFT is a great service to automate your tasks. Many such tasks involved twitter. For example you could you could export your tweets to dropbox or other services. But with this restriction in place, that's not possible anymore. [IFFFT has already removed Twitter based triggers](http://thenextweb.com/apps/2012/09/20/ifttt-removes-twitter-triggers-comply-new-api-policies/). It can still post to twitter but won't use tweets and post them anywhere for you. This restriction isn't new actually but is being enforced strictly now. You can expect more such services to go soon.

### Deprecation of @Anywhere API

[Anywhere API](https://dev.twitter.com/docs/anywhere/welcome) of Twitter allowed you to hyperlink tweets, usernames and twitter related content automatically on a webpage. It was a very easy way to embed twitter based content. But that's not possible as the API has been deprecated. To embed tweets or usernames, you can either use web intents or use the Embed Tweet/Embed Timelines feature.

### Display Requirements

Twitter has published a long [article about how the tweets should be displayed](https://dev.twitter.com/terms/display-requirements). Basically it just tells that you have to replicate the tweet in its entirety as it appears on Twitter.com. You can't remove or add items to a tweet on your own. This may effect many websites. To embed tweets on your website, you can use only the official methods. Some of the display requirements can be irritating like absolute time cannot be displayed on the timeline but only on the individual tweet. Users are already complaining. Check the screenshot of a review posted at Twicca's play store link which recently implemented these requirements.

[![Play Store comment about Twitter Display issues](images/twicca_display_requirements_complaints.png#center "Twitter Display Requirements Issues - Twicca")](https://play.google.com/store/apps/details?id=jp.r246.twicca&reviewId=17537222675960206600)

Also username and both the name of the person should be displayed side by side. On small mobile screens with longer names/ids, this won't look good.

Not only these but there are other type of restrictions too like you can't inform users that a tweet has been unfavorited or deleted. Hotot for Chrome tells you when a tweet gets deleted by marking it separately. So I guess this feature has to go if it has to comply with the guidelines.

### Why is Twitter doing this?

Well Twitter till now had not much revenues to boast of. The only revenues it was getting was from promoted tweets service. To show ads and to become a full media company, it needs to control the content and how its displayed. Also Twitter's official website and clients are not that much uses as much as the 3rd party apps. So by putting a restriction on them, it can direct users to its own website and clients from where it can push ads. Well earning money is good but doing it by alienating the developers and your users is not a good move. Well as of now, Some developers are appealing or even trying to find out workaround these problems. One can only hope that Twitter may relax some of these restrictions but the true effect of these changes will be felt after March 5, 2013 when all these changes come into effect.

### What Twitter could have done?

Instead of alienating and restricting developers, it could have charged the developers to make apps. This would also have helped in restricting malicious apps and would also have helped them earn some revenue while maintaining a healthy developer ecosystem.
