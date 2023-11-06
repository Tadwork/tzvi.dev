---
title: "Too many redirects"
date: 2023-11-06T09:03:20-08:00
draft: true
---
## Too many redirects ???

While completing the transition of this site from Gatsby to Hugo I took the opportunity to transfer the domain from Google Domains (which was shutting down) to Cloudflare. Cloudflare offers additional features on top of dns and domain hosting since it provides a caching layer on top of web sites that can help prevent sites from being overwhelmed. It also offers additional security focused options such as [Always use HTTPS](https://developers.cloudflare.com/ssl/edge-certificates/additional-options/always-use-https/) which redirects all visitor requests from http to https for all subdomains and hosts. 

WHile I had this option turned on originally, not anticipating any downsides, I quickly realized that it didn't play well with the web hosting I was using at Github Pages and AWS CloudRunner which already served the page as HTTPS and was therefore getting pushed into a [Redirect Loop](https://developers.cloudflare.com/ssl/troubleshooting/too-many-redirects/). Once I turned off the option to Always Use HTTPS on the Cloudflare console the redirect loop went away and routing worked as expected.

