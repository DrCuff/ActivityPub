# ActivityPub
Research on ActivityPub

Taking the lead from Scott Hasselman:

https://www.hanselman.com/blog/use-your-own-user-domain-for-mastodon-discoverability-with-the-webfinger-protocol-without-hosting-a-server

```% curl "https://mast.hpc.social/.well-known/webfinger?resource=acct:DrCuff@mast.hpc.social"```

```json

{
  "subject":"acct:DrCuff@mast.hpc.social",
  "aliases":[
    "https://mast.hpc.social/@DrCuff",
    "https://mast.hpc.social/users/DrCuff"
    ],
  "links":[
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
