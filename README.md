# WebFinger

TL;DR:  In the end I just used a simple apache redirect:

```Redirect /.well-known/webfinger https://mast.hpc.social/.well-known/webfinger?resource=acct:drcuff@mast.hpc.social
```


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

More details here from the comments on Scott's blog:

https://blog.maartenballiauw.be/post/2022/11/05/mastodon-own-donain-without-hosting-server.html

https://rpm.sh/custom-mastodon-domain/

You can test this or any other tool with:  

https://webfinger.net

In the end I did a simple apache redirect that made it work as it was only me!

```html
<VirtualHost _default_:443>
    ServerAdmin root@cuff.social
    ServerName cuff.social
#send to mast.hpc.social
    Redirect /.well-known/webfinger https://mast.hpc.social/.well-known/webfinger?resource=acct:drcuff@mast.hpc.social
</VirtualHost>
```


And it worked a treat via https://webfinger.net/lookup/?resource=james%40cuff.social which now goes to https://mast.hpc.social properly.


```Request Log

18:09:01 Looking up WebFinger data for acct:james@cuff.social
18:09:01 GET https://cuff.social/.well-known/webfinger?resource=acct%3Ajames%40cuff.social
JSON Resource Descriptor (JRD)

{
  "subject": "acct:DrCuff@mast.hpc.social",
  "aliases": [
    "https://mast.hpc.social/@DrCuff",
    "https://mast.hpc.social/users/DrCuff"
  ],
  "links": [
    {
      "rel": "http://webfinger.net/rel/profile-page",
      "type": "text/html",
      "href": "https://mast.hpc.social/@DrCuff"
    },
    {
      "rel": "self",
      "type": "application/activity+json",
      "href": "https://mast.hpc.social/users/DrCuff"
    },
    {
      "rel": "http://ostatus.org/schema/1.0/subscribe",
      "template": "https://mast.hpc.social/authorize_interaction?uri={uri}"
    }
  ]
}
```

There's a @james in the root that also forwards the site if anyone picks up the https://cuff.social/@james link, like twitter etc:

```html
<html>
  <head>
    <meta charset="utf-8" />
    <title>Redirecting to https://mast.hpc.social/@drcuff</title>
    <meta http-equiv="refresh" content="0; URL=https://mast.hpc.social/@drcuff">
    <link href="https://github.com/drcuff" rel="me">
    <link href="https://witnix.com/" rel="me">
  </head>
<body style="background-color:black;">
   <a rel="me" href="https://mast.hpc.social/@DrCuff">Mastodon</a>
</body>
</html>
```
