---
title: Get a local copy of this site running
layout: wikistyle
---

Get a local copy of this site running
================

So you can see what your changes look like before pushing a commit.

This Github Pages site for the Concord Consortium organization uses [jekyll](https://github.com/mojombo/jekyll) a Ruby static site generator that is automatically integrated into the GitHub Pages feature. See: http://pages.github.com/ for more information about how GitHub Pages works.

[Install the Ruby Gem Jekyll](https://github.com/mojombo/jekyll/wiki/install)

If you want to be able to see code formatted with [Pygments](http://pygments.org/) also install the Python library Pygments.

Start the local Jekyll server:

    $ jekyll --server

Open the concord-consortium jekyll site in your browser: [http://0.0.0.0:4000](http://0.0.0.0:4000)

The configuration file: `_config.yml` enables automatic regeneration of the site when you make changes to files in this directory.