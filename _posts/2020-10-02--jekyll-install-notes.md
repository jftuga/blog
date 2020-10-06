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

## Running locally on Raspberry Pi 3 with Buster
* [Installing Jekyll on a Raspberry Pi](https://dave.thwaites.org.uk/website/installing-jekyll.html)
* * Updated the ruby version to `2.6.6` in this file: [install_ruby_rpi.sh](https://gist.github.com/blacktm/8302741)

* [Fix for this error](https://github.com/jekyll/jekyll/issues/4268):

```
    Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/styles.scss':
                    Invalid US-ASCII character "\xE2" on line 3
```

1) Remove or replace all non-ascii characters in your blog by running: `grep --color='auto' -P -n "[\x80-\xFF]" *`
2) Follow [these instructions](https://github.com/jekyll/jekyll/issues/4268#issuecomment-167406574) to change the `locale`:

```
# Install program to configure locales
RUN apt-get install -y locales
RUN dpkg-reconfigure locales && \
  locale-gen C.UTF-8 && \
  /usr/sbin/update-locale LANG=C.UTF-8

# Install needed default locale for Makefly
RUN echo 'en_US.UTF-8 UTF-8' >> /etc/locale.gen && \
  locale-gen

# Set default locale for the environment
ENV LC_ALL C.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8
```

___

You'll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

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

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll's GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
