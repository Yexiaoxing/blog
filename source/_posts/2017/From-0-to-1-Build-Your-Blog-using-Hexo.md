---
title: 'From 0 to 1: Build Your Blog using Hexo'
date: 2017-11-19T02:13:50.000Z
categories:
  - Tutorials
tags:
  - Installation
  - Hexo
---

Want to have a place to save your ideas? You may need a blog. But the question comes, what kind of blogging software do you need?

I have tried many blogs, such as WordPress and Ghost. They both require to be installed on a server but I don't want to maintain a server (too lazy). So I come to Hexo, a static blog framework.

In this blog, I will mark down some important steps to set up a Hexo instance locally and to deploy it on GitHub Pages (possibly with CloudFlare or Netlify in next post), with the assumption of using a newly-installed macOS High Sierra.

## Install Packages

### Install Homebrew

Homebrew is a package manager for macOS. It will simplify many things such as finding packages and installing dependencies. Its official website is <https://brew.sh> .

The installation is very simple -- just paste this line at a Terminal prompt:

{% codeblock lang:shell %}
        $ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endcodeblock %}

It will also install Command Line Tools (CLT) for Xcode. This is required for compiling source codes.

After installing homebrew, we can run `brew doctor` to see if it is successfully installed and able to use.

### Check if Git is installed

To make sure git is installed, we can run `command -v git` . If it doesn't complain that "command not found", things are fine.

If, unfortunately, it complains, check if Homebrew is successfully installed, and try `brew install git` to see if it can fix.

### Install Node.JS

Since Hexo itself is a Node.js package, we will need to install Node.js first. Node.js itself updates very frequently, and the Hexo recommends using NVM.

NVM, short for _Node Version Manager_ , is a script to manage Node.js versions. It can also be installed using a single line:

{% codeblock lang:shell %}
    $ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
{% endcodeblock %}

(Note: Please check <https://github.com/creationix/nvm> to see if there is any update to NVM install script.)

Once NVM is installed, restart the Terminal then install a stable version of Node.js:

{% codeblock lang:shell %}
    $ nvm install stable
{% endcodeblock %}

### Install Hexo

You are finally here. Just install Hexo with npm:

{% codeblock lang:shell %}
    $ npm install -g hexo-cli
{% endcodeblock %}

It will install a global instance of Hexo in your home directory.

### Use a Mirror for npm (Maybe for Chinese Users Only)

Emmm... As we all know, the network in China is undergoing some difficulties. If you fail to fetch packages from the official registry (registry.npmjs.org) using NPM, you may try to use one of the mirrors in China.

Here, I recommend you to use the Taonpm (Taobao NPM Mirror). This is run by Alibaba so the speed and availability can be guaranteed.

There are many ways to use it. For example, use the smart-npm package instead of npm, by running `npm install --global smart-npm --registry=https://registry.npm.taobao.org/` . Or alias a new command:

{% codeblock lang:shell %}
    $ alias cnpm="npm --registry=https://registry.npm.taobao.org \
    --cache=$HOME/.npm/.cache/cnpm \
    --disturl=https://npm.taobao.org/dist \
    --userconfig=$HOME/.cnpmrc"
{% endcodeblock %}

(Reference: <http://www.uedbox.com/npm-install-slow-solution/> )

## Initialize Your Blog Locally

Now you have installed Hexo, it is time to initialize Hexo. Simply run it (replacing

<folder> with your desired target):</folder>

{% codeblock lang:shell %}
    $ hexo init <folder>
    $ cd <folder>
{% endcodeblock %}

This creates a basic project folder, which looks like:

{% codeblock lang:shell %}
    \_config.yml # Site configuration file
    package-lock.json # You may see this file which is used by npm
    package.json # Application dependencies
    node_modules # Here store Node.js Modules
    scaffolds # When you create a new post, Hexo bases the new file on the scaffold
    source # Where site contents locate
    themes # Theme folder for static website generation
{% endcodeblock %}

### Configure Your New Blog

You are having a fresh Hexo instance. Why not customize it? You can modify most site settings in `_config.yml` .

Please refer to <https://hexo.io/docs/configuration.html> for available settings. Here I would like to mention some important

**Site:** Title, subtitle, description, author, language, timezone

**URL:** url, root, permalink

## How About... Start Writing?

No one will read an empty blog, right? So, let's start writing! To create a new post or a new page, just run the command:

{% codeblock lang:shell %}
    $ hexo new [layout] <title>
{% endcodeblock %}

There are three layouts available by default, namely `post` , `page` , and `draft` . If you don't specify, `post` layout will be used.

Also, remember to quote your title if it contains spaces or special characters!

You may see that there is a special layout: `draft` . Posts initialized with this layout will not be displayed by default and you can use `publish` command to move drafts to post folder.

After running the command, Hexo will build a new file based on the corresponding file in `scaffolds` file. Yes -- you can add your own!

By default, the generated filename will use the post title. If you want to have a different one, consider editing the setting `new_post_name` . For more information, please read <https://hexo.io/docs/writing.html> .

### Markdown

Hexo uses Markdown in the content. What is Markdown?

{% blockquote Wikipedia <https://en.wikipedia.org/wiki/Markdown> Markdown %}

Markdown is a lightweight markup language with plain text formatting syntax. It is designed so that it can be converted to HTML and many other formats using a tool by the same name.

{% endblockquote %}

It has a very simple but powerful syntax.

{% codeblock lang:markdown %}
    # Heading

    ### Sub-heading

    #### Another deeper heading

    Paragraphs are separated
    by a blank line.

    Two spaces at the end of a line leave a
    line break.

    Text attributes _italic_, *italic*, __bold__, **bold**, `monospace`.

    Horizontal rule:

    ---

    Bullet list:

     * apples
     * oranges
     * pears

    Numbered list:

     1\. apples
     2\. oranges
     3\. pears

    A [link](http://example.com).
{% endcodeblock %}

### Tag Plugins

Hexo provides a useful way for you to quickly add specific content to the posts. For more information, please refer to [https://hexo.io/docs/tag-plugins.html](https://hexo.io/docs/tag-plugins.html#Code-Block) .

### Front-matter

Each post has its own style. You can customize settings for each post in front-matter. Front-matter is a block of YAML or JSON at the beginning of the file, terminated by three dashes when written in YAML or three semicolons when written in JSON.

YAML:

{% codeblock lang:yaml %}
    title: Hello World
    date: 2013/7/13 20:46:25
    ---
{% endcodeblock %}

JSON:

{% codeblock lang:javascript %}
    "title": "Hello World",
    "date": "2013/7/13 20:46:25"
    ;;;
{% endcodeblock %}

There are various settings available, like title, date, updated date, categories, and tags.

Posts are supporting categories and tags. Please read <https://hexo.io/docs/front-matter.html> .

### Images

Want to insert some images? Hexo provides a different mechanism than Markdown itself. You may read <https://hexo.io/docs/asset-folders.html> for more details.

## Take a Look At The Blog

OK, now you have your owned writings. What about taking a look at them? Hexo provides an optional module of a server. To start using the server, you will first have to install hexo-server.

{% codeblock lang:shell %}
    $ npm install hexo-server --save
{% endcodeblock %}

Remember to run it under the root of your blog. After installation, we can now run the server.

{% codeblock lang:shell %}
    $ hexo server
{% endcodeblock %}

Your website will now run at <http://localhost:4000> by default. When the server is running, Hexo will watch for file changes and update automatically so it's not necessary to manually restart the server.

## Generate A Static Site

Well, what if you don't want to run another program to serve your website, instead of using Nginx to host static files? You can actually generate a static one! In Hexo, it can be done in a simple way.

{% codeblock lang:shell %}
    $ hexo generate
{% endcodeblock %}

Then, everything you need is now in `public/` folder. Open `public/index.html` to see. So fantastic.

Wait, why my website becomes a bunch of plaintexts? It is because some browsers don't allow you to load local stylesheets and Javascript. But, no worry. Don't forget that we will push to GitHub Pages later.

### Watch File Changes

You know, it is very annoying to run the generator every time we change any file. Hexo can watch for file changes and regenerate files immediately by comparing the SHA1 checksum. Just run this:

{% codeblock lang:shell %}
    $ hexo generate --watch
{% endcodeblock %}

No pain.

## Themes

Don't like the default look of your blog? There are hundreds of themes available (well I don't count it). You can pick up one of your favorite themes and install it.

Here are the steps.

1. Find a theme here: <https://hexo.io/themes/>
2. Get the GitHub repo address of the theme. For example, I will use <https://github.com/Ben02/hexo-theme-Anatole> .
3. Clone it to `themes/` folder by `git clone https://github.com/Ben02/hexo-theme-Anatole.git themes/anatole`
4. Install required plugins (if stated in the theme description)
5. Update `_config.yml` file. You will see a `theme` configuration. Change the value to the folder name that contains your theme ( `anatole` here).
6. Re-generate your site and take a look.

## Deploy to GitHub

It's now time to publish your site! Excited?

In this example, we will use GitHub Pages. Why?

{% blockquote GitHub Help <https://help.github.com/articles/what-is-github-pages/> What is GitHub Pages? %}

GitHub Pages is designed to host your personal, organization, or project pages directly from a GitHub repository.

{% endblockquote %}

Well, it is free and stable, why not? Let's get started.

(What? You know nothing about Git and GitHub? Google it.)

First, go to GitHub and [create a new repository](https://github.com/new) named _username.github.io_ . This will be your future blog space.

Second, install hexo-deployer-git. Yes Hexo itself doesn't provide it (to keep it simple).

{% codeblock lang:shell %}
    $ npm install hexo-deployer-git --save
{% endcodeblock %}

Then, add some settings to your `_config.yml` :

{% codeblock lang:yaml %}
    deploy:
      type: git
      repo: <repository url, copied from GitHub>
      branch: [branch]
      message: [message]
{% endcodeblock %}

Last, run `hexo deploy` . If nothing goes wrong, you can navigate to http://

<username>.github.io to view your website!</username>

### Bind Your Owned Domain to GitHub Pages

What? You have your owned domain and you want to bind it to your new blog? Of course, it can be done easily.

First, you need to create a new file called `CNAME` under `source/` folder, with the content of your desired domain (including subdomains.

Second, set the DNS record of your domain, CNAME it to `USERNAME.github.io` .

Third, run `hexo clean` then deploy again.

Finally, test your new custom domain.

Please be noted that, if you want to use an APEX domain, you need to follow the instruction here: <https://help.github.com/articles/setting-up-an-apex-domain/>.

## References

Hexo itself actually provides some document here: <https://hexo.io/docs/index.html>. Please read it if you want to know more.
