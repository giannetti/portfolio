---
layout: single
title:  "Site Updates"
date:   2024-06-13 18:04:00 -0400
categories: 
    - jekyll 
    - update
---

I knew there was a reason I deferred website updates until the summer.

My hosting provider started to charge extra for running Ruby applications a while back. This effectively knocked out my Jekyll site generation workflow since I didn't feel like paying. It was fine, I told myself, since I had been meaning to move to GitHub Pages for version control anyway. Previously, each time I updated my site, I effectively clobbered what was there before, which is not the greatest in terms of digital preservation. But! But. Getting everything working on GitHub Pages involved shaving a mountain full of yaks, reminding me that Ruby is kind of the worst. Search is the last thing to fix on this site, so hopefully I'll get that working soon (or just admit failure and mask it all together, lol).

I won't go into all the wrestling with Ruby and Jekyll, except to say that GitHub Pages caps you at Jekyll 3.9.5 and only works with select versions of Ruby (not the latest stable release!).[^fn1] [GitHub Actions](https://jekyllrb.com/docs/continuous-integration/github-actions/) are supposed to be a workaround for using whichever versions you like, but it just didn't work in my experience. Also, Ruby Version Manager (rvm) is such a headache, and I found rbenv to be easier (thanks to the Code4Lib Slack for setting me on the right path).

Probably the biggest unintended consequence of mapping a GitHub repo to my root domain was that all the little side projects and slide decks that were formed as baseurl/project were giving 404s now because everything was pointing to GitHub's servers. Ha ha ha ha ha ha HA HA.. oh.. hmmm. This was also fine, I told myself, because I had been meaning to collect all my slide decks in one place, and you may find them now at <https://slides.francescagiannetti.com/>. As for my never-ending technical experiments and bits and bobs, those have now been moved to <https://sandbox.francescagiannetti.com/>. I have only made public those messes I more regularly need to find (*grins*).

[^fn1]: See <https://pages.github.com/versions/>.