title: Add Your Website to Google Search Console (Webmaster Tools)
photo: v1479280352/photo-1477013743164-ffc3a5e556da_y0vbmo.jpg
tags: [seo, google]
---

This article covers how to add a new property (website) to the Google Search
Console (Webmaster Tools).

## HTTP VS. HTTPS

Google Search Console (Webmaster Tools) treats HTTP and HTTPS as separate sites.

`http://www.example.com` is considered a different URL from
`https://www.example.com` because it may validly serve different content.

When adding a new property, don't forget to add both `HTTP` and `HTTPS` (if
your server is configured for `HTTPS`).

## WWW VS. NON-WWW

Similarly, don't forget to add both `WWW` and `NON-WWW` version of your
website.

Later, you can set in the settings wheter `WWW` or `NON-WWW` is prefered.

## HTTP + HTTPS + WWW + NON-WWW Together

So in the end, you will have four (4) sites (properties).

```
http://www.example.com
https://www.example.com
http://example.com
https://example.com
```

Don't forget to submit the sitemap(s) for all properties.

Happy coding.
