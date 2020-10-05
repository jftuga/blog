---
layout: post
title:  "Jekyll Install and Blogging Notes"
date:   2020-10-02 14:54:27 +0000
categories: jekyll
---

# Ubuntu 20.04 Install Notes

## Jekyll Install
* [Jekyll on Ubuntu](https://jekyllrb.com/docs/installation/ubuntu/)

## GitHub Pages
* create new repo w/ README.md
* change settings
* * GitHub Pages
* * Source: **Branch**:`main` **Folder**:`/ (root)`
* * *Save*
* [Mastering GitHub Markdown](https://guides.github.com/features/mastering-markdown/)

## GitHub + Jekyll
* git clone https://url/.../blog.git
* cd blog
* jekyll new . --force
* bundle exec jekyll serve --host=0.0.0.0 --livereload

## Config
* Modify `_config.yml`: title, description, baseurl, url, etc.
* Modify `Gemfile`: 
* * remove this line: `gem "jekyll"...`
* * uncomment this line: `gem "github-pages"...`

## Ruby Gems
* After the `Gemfile` is modified, run these commands:
* * bundle update
* * bundle install
* * To rebuild the sitr: `bundle exec jekyll build`

## Modifying the Theme
* * Location of *Theme* listed in `_config.yml`: `bundle info --path minima`
* * Copy that subdirectory's `_layout` folder to underneath the current `blog` folder.
* * * This will override the theme's defaults.
* * * Ex: `~/gems/gems/minima-2.5.1$ cp -av _layouts/ ~jftuga/blog/`
* * The same can be done with the other folders: `_includes`, `_sass`, and `assets`
* * * Once this is done, you can remove this line from your `_config.yml` file: `theme: minima`
* * * You also need to remove this line from your `Gemfile` file: `gem "minima", "~> 2.5"`
* * You will now need to rerun: `bundle update`
* * Followed by: `bundle exec jekyll serve --host=0.0.0.0 --livereload --incremental`

## Redirection
* To rediect `https://username.github.io` to`https://username.github.io/blog/`, see the following instructions:
* [Redirecting GitHub Pages](https://gist.github.com/domenic/1f286d415559b56d725bee51a62c24a7)
* You should also set the Jekyll theme to `jekyll-theme-minimal` in the `_config.yml` file

___

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
