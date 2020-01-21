---

---
# About
This repo is the source for https://travleev.github.io. 

# Notes
When two files, `index.html` and `index.md` are present the `html` is used and
no theme is applied (even when the front matter is added). 

## Check locally
Look to this [article](https://help.github.com/en/enterprise/2.14/user/articles/setting-up-your-github-pages-site-locally-with-jekyll). In short:

1. Create `Gemfile` with the following content:
```
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
```

2. Run the command 
```
>sudo apt-get install libghc-zlib-dev
>bundle install
```

1. Start serving with jekyll:
```
>bundle exec jekyll serve
```

## Links to md
Local links can be used, for example {% raw %} `[home]({% link index.md %})` {%
endraw%}, which renders to [home]({% link index.md %}). In order to make this
link in webbrowser to link to an html and not to the original md file (which
than will be downloaded), add to the target md file the front matter -- even
empty. 
