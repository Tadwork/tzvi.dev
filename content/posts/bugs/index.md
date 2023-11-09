---
title: "What is a (software) bug?"
date: 2019-08-08T09:03:20-08:00
featuredImage: "posts/bugs/bugs.webp"
tags: ["software development", "agile", "bug"]
description: "Some guidelines I use when evaluating the criticality of bugs and inconsistencies in software"
---

Creating great software involves a hard balancing act between crafting new features while maintaining the functionality already in place. When something doesn’t go as planned during that process the issue is usually referred to as a “bug.” While this is a handy term, it is often used to include a broad definition of problems and inconsistencies and that makes prioritizing work based on that term hard. Do you focus on fixing bugs as soon as possible? Does every bug have to go through a lengthy review process? Recently, I came to the realization that bugs in software share a lot in common with their _Hexapoda Invertabre_ namesake.

Some bugs, like midges, are harmless but annoying. Others, such as African honey bees can be extremely dangerous or even deadly. For that reason, it is impossible to lump all “bugs” together when prioritizing how to address them. Over time I have begun to form a set of guidelines in order to help classify software bugs and identify how quickly they need to be addressed.

## 🐝 Critical Bugs

The genus _“Softwarus Criticalus”_ is one of the more dangerous classes of bugs. These bugs need to be worked on as soon as they are found. This often means that work on newer features will be disrupted while they are addressed but that is a worthwhile trade-off. Bugs in this genus can be identified by looking at the following traits:

If a problem was discovered in production and is **preventing the core use** of the application.
If a problem was discovered in testing before an application was released but will **disrupt the core use** of the feature being released.
A feature was never implemented correctly as described by the “Acceptance Criteria.” For this last family of bugs, it is often easier to link to the original feature description or ticket so it is clear what is missing
We found that these bugs need to be addressed during the sprint and should not wait for grooming.

## 🐜 Usability Bugs

Some bugs are annoying and potentially harmful but do not need to be addressed immediately. This bugs should also be addressed quickly and exterminated before they get too comfortable in their new home. With these bugs, it is often best to discuss why they are there to prevent them from coming back in the future. This discussion can happen as part of the normal development process so that design, product, and developers can identify a permanent fix. This genus, which I call _“Softwarus Difficultis,”_ contains the following families of bugs:

A defect was found in a rarely used component or workflow in production. The use case is non-standard and a workaround can be communicated to users while a permanent fix is found.
A feature was completed as described but subsequent user testing before release to production has shown that some changes need to be made before it can be released.
These bugs are groomed and discussed during the normal sprint cadence or prioritized by the Team Lead for more urgent action _before the start of a new sprint_.

## 🐞 Inconsistency Bugs
The last category of bugs often includes some of the more annoying families of bugs. These bugs make the experience inconsistent and/or unprofessional. Fixing these bugs is an important part of ensuring the long-term success of the software but it is not critical to do so right away. Some examples of these bugs are:

1. Inconsistently copy, colors, spacing, etc.
2. An update to a feature completed correctly by developers but not yet released to production to covers infrequent use cases.

We found that it was best if these bugs flow through the normal sprint process so that representatives from product, design, and tech can agree on the right way to fix it.

Does your team have another way they categorize and prioritize bugs? If you have suggestions or want to tell me how your team tackles this problem I would love to hear more by responding below.

_This article was originally posted on [Medium](https://medium.com/@tadwork/what-is-a-bug-ccccf7a84fdd)_