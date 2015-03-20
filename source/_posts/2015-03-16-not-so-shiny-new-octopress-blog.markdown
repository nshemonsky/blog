---
layout: post
title: "Not So Shiny New Octopress Blog!"
date: 2015-03-20 10:25:07 -0400
comments: true
categories: [meta, octopress]
---

After much neglect, I am resurrecting my blog. It's been long overdue and after
reading some inspirational posts elsewhere on the Internets, specifically 
[On Blogging][] by [beerops][], I've decided it was time.

In this reincarnation, I've switched from Wordpress to [Octopress][], mostly to
simplify the posting process. With that decision came moving away from my 
previous hosting provider as well. Both my main landing page of 
[www.nickshemonsky.com][www] and [blog.nickshemonsky.com][blog] are now hosted 
on github-pages.

As originally intended, this blog will serve a few purposes. 

1. As a technical reference. Googling always works but sometimes I just need a 
   place to take a quick note. Hopefully, these help someone else out down the 
   line.
2. As a public roadmap for mostly professional goings-on.
3. As a place to comment on beers I've sampled. At this point, I'm well over 
   500 and would like to start documenting my thoughts on what I've tasted. If 
   you're on [Untappd][], add me as a friend! 
4. As a place to rant, rave and offer commentary on whatever happens to be on my
   mind.

With the interest of #1 in mind from the list from above, I wanted to share the
few quick mods I made to the Octopress configuration.

### Removing the rss subscription icon

First, modify `_config.yml` removing `/atom.xml` from the the subscribe_rss
line.

```
# RSS / Email (optional) subscription links (change if using something like Feedburner)
subscribe_rss:
subscribe_email:
# RSS feeds can list your email address if you like
email:
```

Then modify `source/_includes/navigation.html` to conditionally include/exclude
the rss icon.

{% raw %}
```
{% if site.subscribe_rss %}
  <ul class="subscribe" data-subscription="rss{% if site.subscribe_email %} email{% endif %}">
    ...
  </ul>
{% endif %}
```
{% endraw %}

### Modifying the date format for posts

The date format for posts can be adjusted via `source/_includes/post/date.html`.
I prefer to not have the time listed for posts.

Original:

{% raw %}
```
{% if page.date %}{% capture time %}{{ page.date_time_html }}{% endcapture %}{% endif %}
{% if post.date %}{% capture time %}{{ post.date_time_html }}{% endcapture %}{% endif %}

{% if page.updated %}{% capture updated %}{{ page.date_time_updated_html }}{% endcapture %}{% endif %}
{% if post.updated %}{% capture updated %}{{ post.date_time_updated_html }}{% endcapture %}{% endif %}
```
{% endraw %}

Modified:

{% raw %}
```
{% if page.date %}{% capture time %}{{ page.date_html }}{% endcapture %}{% endif %}
{% if post.date %}{% capture time %}{{ post.date_html }}{% endcapture %}{% endif %}

{% if page.updated %}{% capture updated %}{{ page.date_updated_html }}{% endcapture %}{% endif %}
{% if post.updated %}{% capture updated %}{{ post.date_updated_html }}{% endcapture %}{% endif %}
```
{% endraw %}

For additional date formatting options see: https://github.com/octopress/date-format

### Removing the simple_search bar

The simple_search is controlled by the {% raw %}
`{% if site.simple_search %}{% endif %}` {% endraw %} conditional in 
`source/_includes/navigation.html`.

Removing the url from the simple_search field in `_config.yml` will remove the
search bar.

### Removing the default /blog directory

Octopress deploys archives and categories  to `/blog` by default. It can be
removed using these steps:

* Replace all occurences of `/blog` with `/` in `_config.yml`
* Move `source/blog/archives` to `source/` 
* Delete `/blog`
* Edit `source/_includes/custom/navigation.html`, replacing `/blog/archives` with `/archives`
* Edit `source/index.html`, replacing `/blog/archives` with `/archives`


[beerops]: https://twitter.com/beerops
[On Blogging]: http://beero.ps/2015/02/08/on-blogging/
[www]: http://www.nickshemonsky.com
[blog]: http://blog.nickshemonsky.com
[Octopress]: https://github.com/imathis/octopress
[Untappd]: https://untappd.com/user/nshemonsky
