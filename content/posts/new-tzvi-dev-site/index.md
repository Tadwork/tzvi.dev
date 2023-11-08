---
title: "Overcoming Bit-Rot on Tzvi.dev"
date: 2023-11-07T09:03:20-08:00
featuredImage: "posts/new-tzvi-dev-site/featured.png"
---
## Revitalizing Tzvi.dev

At the beginning of this month, I initiated a project to revive my personal web domain, [tzvi.dev](https://www.tzvi.dev), after a two-year period of inactivity. The latest version of the site was constructed using the Gatsby CMS, a framework based on React designed for building static websites. This choice seemed ideal back then due to my proficiency with React and TypeScript.

Regrettably, the decision proved to be less sustainable over time. When I attempted to reinstall the site's dependencies after two years, I encountered numerous errors, such as:

```
npm ERR! Could not resolve dependency:
npm ERR! peer eslint-plugin-react-hooks@"1.x || 2.x" 
...
```

Trying to resolve these issues by selecting alternative dependencies only led me deeper into a maze of compatibility problems often called 'dependency hell'.

This challenge, where neglected code becomes unworkable due to a lack of updates, is sometimes known as 'bit-rot'. It felt like the code decayed as if by itself over time. I quickly realized that it would be more efficient to replace the outdated Gatsby framework with a more straightforward alternative.

## Discovering Hugo

![Hugo logo](./hugo-logo-wide.svg)

I conducted some research and chose [Hugo](https://gohugo.io/) to replace Gatsby. Hugo is a static site generator developed in Go, notable for its rapid build times and user-friendly API. Hugo's creators tout that it will "make building websites fun again," which was a welcome perspective after my previous struggles. Although direct conversion from Gatsby to Hugo wasn't possible, I opted for a fresh start and selected a simple theme for customization to resemble the former design.

After theme installation, Hugo's effectiveness and enjoyable experience became clear. Themes are added as Git submodules, simplifying the process of delving into the theme's code for better understanding. For any custom changes, it was as simple as duplicating the relevant file into my source code and adjusting as needed.

For development, I ran `hugo server --buildDrafts`, which built a local preview of the site that updated automatically with each modification to the source files. This efficient feedback loop simplified the editing process, enabling me to complete the transition in just one day.

To publish the site, I followed [a straightforward guide](https://gohugo.io/hosting-and-deployment/hosting-on-github/) that instructed me on using a GitHub Action for the final build on GitHub Pages.

## The Outcome

![Old vs New](./compare-old-new.png)

As illustrated in the screenshot above, the old and new versions of the site look remarkably alike. Now that the transition is complete, I am quite pleased with the outcome and would suggest Hugo to others for its adaptability and ease of use.

The full source code for the site is hosted on [GitHub](https://github.com/Tadwork/tzvi.dev) for anyone interested in a more in-depth examination.

## Addendum: Redirect Complications

![Redirect loop](./redirects.png)

Following the site overhaul, I transferred the domain from Google Domains to Cloudflare. Known for its DDoS protection, Cloudflare also provides DNS and domain services at competitive rates. Post-transfer, I enabled several of Cloudflare's security features, including [Always Use HTTPS](https://developers.cloudflare.com/ssl/edge-certificates/additional-options/always-use-https/), which automatically redirects HTTP requests to HTTPS for better security.

Initially, I hadn't foreseen any issues with this setting. However, it soon became apparent that it was incompatible with my hosting services at GitHub Pages and AWS CloudRunner, which already used HTTPS. This caused a [Redirect Loop](https://developers.cloudflare.com/ssl/troubleshooting/too-many-redirects/), leading to website access problems. After some troubleshooting, I disabled the "Always Use HTTPS" option in Cloudflare, which resolved the redirect loop and restored proper routing.