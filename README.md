# WebFinger
Research on ActivityPub, starting with WebFinger, let's see if we can make a php script starting with the background.

Taking the lead from Scott Hanselman's epic post here:

https://www.hanselman.com/blog/use-your-own-user-domain-for-mastodon-discoverability-with-the-webfinger-protocol-without-hosting-a-server

First up, let's get our current "finger".

```% curl "https://mast.hpc.social/.well-known/webfinger?resource=acct:DrCuff@mast.hpc.social"```

```json

{
  "subject":"acct:DrCuff@mast.hpc.social",
  "aliases":
  [
    "https://mast.hpc.social/@DrCuff",
    "https://mast.hpc.social/users/DrCuff"
  ],
  "links":
  [
    {
      "rel":"http://webfinger.net/rel/profile-page",
      "type":"text/html","href":"https://mast.hpc.social/@DrCuff"},
    {
      "rel":"self","type":"application/activity+json",
      "href":"https://mast.hpc.social/users/DrCuff"
    },
    {
      "rel":"http://ostatus.org/schema/1.0/subscribe",
      "template":"https://mast.hpc.social/authorize_interaction?uri={uri}"
    }
  ]
}
```

Note php must return: ```application/jrd+json```
