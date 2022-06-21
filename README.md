# hyrtsi.github.io

# About

- This is the repo for my Jekyll blog, [hyrtsi.github.io](https://hyrtsi.github.io)
- It's deployed from `docs/` directory of `master` branch


# Installation and local testing (for developers)

Developed and tested using Ubuntu 20.04. The github pages are updated automatically upon each commit so these steps are only for local testing.

1. Install dependencies

- Ruby
- Jekyll
- Bundler

```
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

`ruby -v` to check your current version. I use `2.7.0p0`

Install jekyll and bundler: 
`sudo gem install jekyll bundler`

`gem -v`, I use `3.1.2`

2. Clone this repo
3. `cd docs`
4. `jekyll new --skip-bundle .`
5. `sudo nano Gemfile`, check that everything is OK like explained [here](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
6. `bundle install`
7. `bundle exec jekyll serve`
8. Test by opening `http://127.0.0.1:4000/` on your browser


## Useful pages

- [Testing Github pages locally](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
- [Jekyll](https://jekyllrb.com/docs/installation/)
- [Ruby](https://www.ruby-lang.org/en/downloads/)
- [Creating Github pages with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
- [Static site generators](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#static-site-generators)
- [Gh pages + Jekyll main](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll)

## Problems

If there are problems, verify your Ruby, RubyGem, Bundler and Jekyll installations.
