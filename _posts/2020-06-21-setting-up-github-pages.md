---
layout: post
author: Hector SB
---
# Setting Up Github Pages
This little tutorial assumes you are using linux.

## Online
### Create a Github Repository for Github Pages
The first thing you will need is to have a repository where to save all the websit. For that, you can follow [github getting started](https://guides.github.com/features/pages/) guide. Once you have created the repository with the correct name, we are ready to start writting in our computer.

## Local
We can write and preview post in our local machine.

### Install Local Dependencies

#### Installing RVM
Jekyll is written in ruby. Thus, we need to get it in our computer. To avoid ruining the version that comes with our system (if any), we are going to use the [RVM (Ruby Version Manager)](https://rvm.io/), which will allow us to have multiple versions of ruby.

As of June 2020, the required steps for installation are the following.

```bash
gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
```

Now, open a new terminal and check its version and the available Ruby versions.

```bash
$ rvm --version
rvm 1.29.10 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io]

$ rvm list
# => - current
# =* - current && default
#  * - default
```


#### Installing Ruby
As we can see, we still do not count with any version of ruby (managed by rvm). Check in the [ruby website](https://www.ruby-lang.org/en/) a resent version and download it with the following command.

```bash
$ rvm install 2.6.3 # Install ruby 2.6.3, select whatever version you want.
```

If we list all the ruby versions again, we will now find it there. Set this version as the ruby version we will be using.


```bash
$ rvm list
ruby-2.6.3 [ x86_64 ]

# => - current
# =* - current && default
#  * - default

$ rvm use 2.6.3
Using /home/hector/.rvm/gems/ruby-2.6.3
```


#### Install Bundler
We use [Bundlere](https://rvm.io/integration/bundler) to handle the dependencies required by ruby to make Jekyll work. As of June 2020, the command required to install it is the following.

```bash
gem install bundler
```


### Clone repository
Clone the repository in your local machine. We will use the home directory as reference for this post.
```bash
$ cd ~ # Change directory to home
$ git clone <repository_url> # Clone our github pages repository
$ cd <repository_folder_name>
```

### Get Ruby Dependencies
Reading the Jekyll documentation for [Github Pages](https://jekyllrb.com/docs/github-pages/), it mentions to add to lines to our `Gemfile`. Note that this file may actually not exists just yet. So we should create it.

```bash
$ touch Gemfile # Create the Gemfile file
```

Now open the file with whatever text editor of your choice and add these lines.
```
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
```

Note that the second line starting with `gem` indicates that it is a plugin required by our project. Once we have saved the changes, install them with the following command.

```bash
bundle install
```

We may encounter the message below after installing the `githu-pages` gem, and what is saying to us is that there are more dependencies required for our project. If we go to `https://github.com/jch/html-pipeline#dependencies` we will see a list of dependencies.

```
Post-install message from html-pipeline:
-------------------------------------------------
Thank you for installing html-pipeline!
You must bundle Filter gem dependencies.
See html-pipeline README.md for more details.
https://github.com/jch/html-pipeline#dependencies
-------------------------------------------------
```

As of June 2020, the `Gemfile` after adding the dependencies for `html-pipeline` would look as follow.

```
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
gem "rinku"
gem "escape_utils"
gem "email_reply_parser"
gem "gemoji"
gem "commonmarker"
gem "escape_utils"
gem "sanitize"
gem "rouge"
gem "RedCloth"
```

Now we  have to install them.

```bash
$ bundle install
```

### Preview Local Site
We have installed all we need to preview or github page locally. Use the following command to run a server to see it in your browser.

```bash
$ bundler exec jekyll serve --watch
```

You should see something like the following message in the terminal. Do not care for the GitHub Metadata warning, it will show up because it is running outside a github instance.
```
Your Gemfile lists the gem escape_utils (>= 0) more than once.
You should probably keep only one of them.
Remove any duplicate entries and specify the gem only once.
While it's not a problem now, it could cause errors if you change the version of one of them later.
Configuration file: /home/hector/hector-sab.github.io/_config.yml
            Source: /home/hector/hector-sab.github.io
       Destination: /home/hector/hector-sab.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
   GitHub Metadata: No GitHub API authentication could be found. Some fields may be missing or have incorrect data.
                    done in 0.474 seconds.
 Auto-regeneration: enabled for '/home/hector/data/sources/hector-sab.github.io'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
[2020-06-21 12:12:10] ERROR `/favicon.ico' not found.
```

Now go to `http://127.0.0.1:4000` in your web browser and behold your brand new blog.

## Content Creation
### Create a Post
Create a folder named `_posts` and create a file with the format `YYYY-MM-DD-title.md`.
```bash
$ mkdir _posts
$ touch _posts/2020-06-20-testing-post.md
```

Now open the `2020-06-20-testing-post.md` file with your favorite text editor and add the following content.
```markdown
# This is Title
Hello world! This is some text.

## This is a subtitle
Here we have more text.
```

[About posts in jekyllrb.com](https://jekyllrb.com/docs/step-by-step/08-blogging/#list-posts).
### Creating a Layout
Layout for posts do not exist, so we have to create it. Create `_layouts` folder with a `post.html` file in it.

```bash
$ mkdir _layouts
$ touch _layouts/post.html
```

Now open `post.html` and add the following content.

```
---
layout: default
---
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
    {{ content }}
  </body>
<html>
```

### Creating Navigation menu
You will also notice that there is no way of navigating to the posts. For a quick fix, you can create a `blog.html` in your root directory containing the following.

```
---
layout: default
title: Blog
---
<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      {{ post.excerpt }}
    <li>
  {% endfor %}
</ul>
```

Create a `_data` forder and a `navigation.yml` file in it.

```bash
mkdir _data
touch _data/navigation.yml
```

Fill it with the following content.

```
docs_list_title: Navigation
docs:
  - name: Home
    link: /
  - name: About
    link: /about.html
  - name: Blog
    link: /blog.html
```

Now, add the following code at the top of your `index.md` so it shows the navigation.

```
<html>
    <h2>{{ site.data.navigation.docs_list_title }}</h2>
    <ul>
      {% for item in site.data.navigation.docs %}
        <li><a href="{{ item.link }}">{{ item.name }}</a></li>
      {% endfor %}
    </ul>
</html>
```

We can do the same for the post layout. 

```
---
layout: default
---
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    <h2>{{ site.data.navigation.docs_list_title }}</h2>
    <ul>
      {% for item in site.data.navigation.docs %}
        <li><a href="{{ item.link }}">{{ item.name }}</a></li>
      {% endfor %}
    </ul>

    {{ content }}
  </body>
<html>
```


Congratulations! You have a blog up and running! It's minimum and kind of ugly, but hey, at least it works. Now you can go and explore how to make it more beautiful by yourself.
