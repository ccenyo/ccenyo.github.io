### ccenyo.github.io - Cenyo Medewou personal website

This project is my personal website. It will serve as a site to hold all my projects and article i write on my journey for being better developper.

I setup the website by using developr-jekyll template which is a beautifull template, simple to use and setup. you can find it  [here](https://github.com/sujaykundu777/devlopr-jekyll)


### Setup and Run

If you want to make a similare website, you can follow the tutorial on the original project [here](https://github.com/sujaykundu777/devlopr-jekyll) or fork this project and customize it at your wish.


> **Step 1.** make sure you install Ruby and Gem on your system

For ruby :

```bash
$ ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux-gnu]
```
For bundler :

```bash
$ gem install bundler
$ bundler -v
Bundler version 2.2.29
```
> **Step 2.** if it's not already done, add jekyll :

```bash
$ bundle update
$ bundle add jekyll
```
 This command will add the Jekyll gem to our Gemfile and install it to the ./vendor/bundle/ folder.

You can check the jekyll version

```
$ bundle exec jekyll -v
jekyll 4.2.0
```

> **Step 3.** Install the gem dependencies by running the following command

```bash
$ bundle update
$ bundle install
```

> **Step 7.** Serve the site locally by running the following command below:

```bash
$ bundle exec jekyll serve --watch
```
or you can also serve using :

```bash
$ jekyll serve
```
