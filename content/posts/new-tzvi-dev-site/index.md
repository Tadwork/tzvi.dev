---
title: "Overcoming Bit-Rot on Tzvi.dev"
date: 2023-11-07T09:03:20-08:00
featuredImage: "posts/new-tzvi-dev-site/featured.png"
---

## Trying to Revive the Old Version

Earlier this month, I embarked on a digital archaeology project—resurrecting [tzvi.dev](https://www.tzvi.dev), my personal web domain, after it lay dormant for over two years. The site's most recent iteration had been built on the Gatsby CMS, a React-based framework for developing static websites. At the time, this was a great choice since I was very familiar with React and TypeScript.

Unfortunately, this decision did not age well. When I tried to reinstall the dependencies two years later, I was met with errors such as:

```
npm ERR! Could not resolve dependency:
npm ERR! peer eslint-plugin-react-hooks@"1.x || 2.x" 
...
```

Attempting to fix this by choosing different dependencies only led me further down the rabbit hole of dependency hell.

This phenomenon, where unmaintained code becomes impossible to deploy if not updated periodically, is sometimes referred to as 'bit-rot' because the code deteriorates on its own over time. After a brief struggle, it quickly became apparent that I could spend my time more effectively by replacing the Gatsby version with something that had fewer dependencies.

## Introducing Hugo

![Hugo logo](./hugo-logo-wide.svg)

After some quick research, I settled on [Hugo](https://gohugo.io/) as a replacement for Gatsby. Hugo is a static site generator written in the Go programming language. On its homepage, the authors claim that using it will "make building websites fun again." While there was no automatic conversion from Gatsby to Hugo, I found a minimalistic theme that I could easily customize to match the previous design.

I realized how powerful Hugo was once I installed the theme. Themes in Hugo are installed by adding a Git submodule, which made it easy to dive into the theme's source code to understand it better. If I needed to make any customizations, I simply had to create a parallel copy of the file in my source code and make the necessary changes.

During development, I simply ran the command `hugo server --buildDrafts` to build a local version of the site that auto-updated with every change to the source files. Once satisfied, I turned to [this simple guide](https://gohugo.io/hosting-and-deployment/hosting-on-github/), which walked me through the steps to add a GitHub Action as a source for the final GitHub Pages build.

## Too Many Redirects

![Redirect loop](./redirects.png)

After completing the transition, I took the opportunity to transfer the domain from Google Domains to Cloudflare. Cloudflare offers additional features on top of DNS and domain hosting, including DDoS mitigation by adding a caching layer on top of websites, which can help prevent sites from being overwhelmed. Additionally, Cloudflare offers security-focused options, such as [Always Use HTTPS](https://developers.cloudflare.com/ssl/edge-certificates/additional-options/always-use-https/), which redirects all visitor requests from HTTP to HTTPS for all subdomains and hosts.

While I originally turned this option on, not anticipating any downsides, I quickly realized that it didn't play well with the web hosting I was using at GitHub Pages and AWS CloudRunner, which already served the page as HTTPS. This resulted in a [Redirect Loop](https://developers.cloudflare.com/ssl/troubleshooting/too-many-redirects/). Once I turned off the "Always Use HTTPS" option in the Cloudflare console, the redirect loop disappeared, and routing worked as expected.

## Results

![Old vs New](./compare-old-new.png)

As you can see from the screenshot above, the old and new sites are actually quite similar. Now that the transition is done, I am quite happy with the results and would highly recommend Hugo for its versatility and user-friendliness.

The site's full source code is available on [GitHub](https://github.com/Tadwork/tzvi.dev) for anyone who wants to take a closer look.