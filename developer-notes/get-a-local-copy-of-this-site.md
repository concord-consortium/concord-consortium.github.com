---
title: Get a local copy of this site running
layout: wikistyle
---

<p>
<a href="#get_a_local_copy_of_this_site_running">Get a local copy of this site running</a><br>
<a href="#install_pip_and_pygments_on_mac_os_x">Install pip and Pygments on Mac OS X</a><br>
<a href="#using_pygments_for_syntax_highlighting">Using Pygments for syntax highlighting</a>
</p>

## Get a local copy of this site running

So you can see what your changes look like before pushing a commit.

This Github Pages site for the Concord Consortium organization uses [jekyll](https://github.com/mojombo/jekyll) a Ruby static site generator that is automatically integrated into the GitHub Pages feature. See: [pages.github.com](http://pages.github.com/) for more information about how GitHub Pages works.

[Install the Ruby Gem Jekyll](https://github.com/mojombo/jekyll/wiki/install)

Install the Python library [Pygments](http://pygments.org/) using pip.

    $ pip install pygments

Start the local Jekyll server:

    $ jekyll --server

Open the concord-consortium jekyll site in your browser: [http://0.0.0.0:4000](http://0.0.0.0:4000)

The configuration file: `_config.yml` enables automatic regeneration of the site when you make changes to files in this directory.

## Install pip and Pygments on Mac OS X

[pip](http://www.pip-installer.org/en/latest/index.html) is a Python package manager analogous to Ruby Gems. Here's one way to install pip and then use pip to install pygments on Mac OS X.

I already have Python 2.7.1 installed using HomeBrew:

    brew install python

Update [setuptools](http://pypi.python.org/pypi/setuptools#cygwin-mac-os-x-linux-other)

    cd python/src/
    wget http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11-py2.7.egg#md5=fe1f997bc722265116870bc7919059ea
    sh setuptools-0.6c11-py2.7.egg

Install and run `get-pip`:

    curl -O https://raw.github.com/pypa/pip/master/contrib/get-pip.py
    python get-pip.py

Use pip to install pygments:

    pip install pygments

Now the pygmentize library is installed:

    $ which pygmentize
    /usr/local/share/python/pygmentize

## Using Pygments for syntax highlighting

Pygments supports syntax highlighting for over [100 languages and markup formats](http://pygments.org/languages/).

Wrap the code to be highlighted with the lines:

    {{ '{%' | escape }} highlight language %}
    code ...
    {{ '{%' | escape }} endhighlight %}
    
Replace **language** with the shortname for the language from one of the available [Pgyments lexers](http://pygments.org/docs/lexers/).

### Pygments Syntax Highlighting Examples

#### Ruby

{% highlight ruby %}
# Recursively converts the keys in a Hash to symbols.
# Also converts the keys in any Array elements which are 
# Hashes to symbols.
module HashExtensions
  def recursive_symbolize_keys
    inject({}) do |acc, (k,v)|
      key = String === k ? k.to_sym : k
      case v
      when Hash
        value = v.recursive_symbolize_keys
      when Array
        value = v.inject([]) do |arr, e|
          arr << (e.is_a?(Hash) ? e.recursive_symbolize_keys : e)
        end
      else
        value = v
      end
      acc[key] = value
      acc
    end
  end
end
Hash.send(:include, HashExtensions)
{% endhighlight %}

#### Ruby irb console session

{% highlight irb %}
>> a = [1, 2, 3]
=> [1, 2, 3]
>> a.class
=> Array
>> s = a.join("\n")
=> "1\n2\n3"
>> s.class
=> String
{% endhighlight %}

#### diff

{% highlight diff %}
diff --git a/meth-link-7056328.patch b/meth-link-7056328.patch
--- a/meth-link-7056328.patch
+++ b/meth-link-7056328.patch
@@ -43,7 +43,7 @@
      }
    }
 
-+  if (found_klass() == NULL && cpool->has_preresolution()) {
++  if (found_klass() == NULL && !cpool.is_null() && cpool->has_preresolution()) {
 +    // Look inside the constant pool for pre-resolved class entries.
 +    for (int i = cpool->length() - 1; i >= 1; i--) {
 +      if (cpool->tag_at(i).is_klass()) {
{% endhighlight %}

#### Apache vhost

{% highlight apache %}
<VirtualHost webgl-matrix-benchmarks.local:80>
   ServerName webgl-matrix-benchmarks
   DocumentRoot /Users/stephen/dev/test/webgl-matrix-benchmarks
   PassengerEnabled off
   <Directory /Users/stephen/dev/test/webgl-matrix-benchmarks >
     Options +Indexes +FollowSymLinks +MultiViews +Includes
     AllowOverride All
     Order allow,deny
     Allow from all
     DirectoryIndex matrix_benchmark.html
  </Directory>
</VirtualHost>
{% endhighlight %}
