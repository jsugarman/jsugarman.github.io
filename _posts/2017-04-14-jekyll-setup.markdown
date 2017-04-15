---
layout: post
title:  "Setting up this site"
date:   2017-04-14 23:30:00:00 +0100
categories: jekyll setup
---

A front end developer colleague mentioned github pages to me and I thought I'd set one up one evening as a
change from my usual Ruby/Rails work. After creating the basic github page available to all github users
I wanted some way to easily generate some pleasantly styled content without having to, god-forbid, write some
html/css. As a rubyist I was pleasantly surprised to discover Jekyll and further pleasantly surprised to discover
that it was pretty simple to implement locally and push to github/master.

However, changing the theme from the default, `minima`, proved rather trickier, so I thought I'd share my resolution of this.

First up was following the instructions available on the [Adding a Jekyll theme to your github pages site](https://help.github.com/articles/adding-a-jekyll-theme-to-your-github-pages-site/#adding-a-jekyll-theme-to-your-site). This works for the `minima` theme out of the box but when trying to use one of the other [Github Pages supported themes][supported-themes] the `jekyll build` command starts to throw errors complainging about missing `home` page and missing `_includes/icon-github.svg`.

It turns out that following the README for a specific supported theme, such as [jekyll-theme-hacker](https://github.com/pages-themes/hacker), requires a bit of gem dependency resolution too. Using `Jekyll v3.4.3+` and `github-pages v39` does not do the job.

# Create github page with Jekyll

To create a Jekyll Github-Pages site locally using you can use [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/). Alternatively, and assuming you have a functional ruby environment, you can create github repository under your own user (or organization) called `<username>.github.io` and then...

* clone it locally

{% highlight shell %}
git clone git@github.com:<username>/<username>.github.io.git
{% endhighlight %}

* navigate to its directory

{% highlight shell %}
cd <username>.github.io
{% endhighlight %}

* install jekyll

{% highlight shell %}
gem install jekyll
{% endhighlight %}

* generate jekyll site (use `--force` if you already have some files in the repo that Jekyll needs to create itself)

{% highlight shell %}
jekyll new . --force
{% endhighlight %}

* run the local jekyll server

{% highlight shell %}
jekyll serve
{% endhighlight %}

* view the site at [localhost:4000](http://localhost:4000)

# Implementing hacker theme

* change `_config.yml` to add the theme

{% highlight YAML %}
# _config.yml
theme: jekyll-theme-hacker
{% endhighlight %}

You can change the theme to any other [supported theme][supported-themes] by changing the last term to the theme name (e.g. midnight, cayman, dinky, etc)

* add github-pages gem to your `Gemfile` using version 134 (latest at time of writing, don't use 39). This gem will include all [supported themes][supported-themes]

{% highlight ruby %}
# Gemfile
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.6"
  gem 'github-pages', '~> 134'
end
{% endhighlight %}

* then bundle install that...

{% highlight ruby %}
bundle install
{% endhighlight %}

At this point the theme will not kick-in because `_layouts/default.html` exists in your site and therefore takes precedence over the `default.html` of the theme. To prevent this you simply...

* rename or delete default.html

{% highlight shell %}
mv _layouts/default.html _layouts/default_ORIGINAL.html
{% endhighlight %}


* view the site at [localhost:4000](http://localhost:4000), assuming you are running the server using `jekyll serve`


[supported-themes]: https://pages.github.com/themes/
