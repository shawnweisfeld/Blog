---
layout: post
title:  "My Way: My Blog"
date:   2017-09-28 16:00:00 -0500
categories: [my-way]
tags: [tools, blog]
comments: true
---
In this 'My Way' post I am going to discuss what tools I use to build my blog and why.

I get many questions, How did you do that? Why did you choose those technologies? 'My Way' posts are designed to share the how and why I did something. They are not a recommendation to follow my lead, your needs might differ from mine and 'My Way' might be complete looney tunes.

# Goals

I had a few main goals when selecting the technology for my blog.

- First it has to be easy to write and post code. With my old blogging platform I spent too much time formatting things into HTML and then messing around with posting files/images. The easier I could make the posting process, the more likely I was to actually post.
- Second, I was looking for something that did not require any admin time. Any time I spent doing admin work was time that I was not writing.

# Selection

There are many blogging platforms out there, perhaps the most popular is WordPress. While super powerful, it requires a web server, a database, a pile of PHP, and constant upgrades. I wanted simple. As a developer I use GitHub allot to post my code repositories and through my travels found that they offer a service called [GitHub Pages](https://pages.github.com/). GitHub Pages was originally designed to host the docs for GitHub repositories, many folks had started using them for blogs.

This feels like the perfect solution:

- GitHub will do all the hosting, so no servers, databases, etc. to setup and manage.
- You write new posts in [markdown](https://en.wikipedia.org/wiki/Markdown) so it is easy to write posts, to include code, files and images.
- Publishing is as simple as checking into a GitHub repository.

# The Setup

I do all my dev work on my Windows PC (actually a Surface Book). So I wanted a tool chain that felt natural. I choose [Visual Studio Code](https://code.visualstudio.com/) to write all my markdown in. It's free, light weight, and super powerful.

I also wanted to be able to run my blog locally, to preview what things looked like prior to publishing to GitHub. For that I chose to use the Windows Subsystem for Linux to install the Jekll tool chain. Ok, lets get started.

## Install Windows Subsystem for Linux

There are good instructions on how to do this [here](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).

## Install VS Code

Super easy install, just press the big green button [here](https://code.visualstudio.com/).

VS Code has an integrated terminal. This is super handy, as it keeps everything I need in one tidy window. Instructions on how to turn it on are [here](https://code.visualstudio.com/docs/editor/integrated-terminal).

## Install Jekyll

Since you write your blog posts in markdown, something is needed to convert that to html. GitHub Pages uses a one time compile to convert your markdown to html every time you check into to your GitHub Repository. However if we want to preview everything locally before we push it we will need to do that compile locally. This is what Jekyll does. 

The Win 10 Bash install instructions are [here](https://jekyllrb.com/docs/windows/#installation-via-bash-on-windows-10).

> NOTE: I followed the steps up to the command to run `jekyll new`. Before you run this command be sure you are in the folder on your Windows partition where you want your project to live. I put mine on my Windows partition so that I can open it in VS Code running on Windows. This also required me running the `jekyll new` command with elevated permissions like this `sudo jekyll new my_blog`.

## Configuring our blog

Now that the scaffolding for our blog is all done, we can open it up in VS Code. Once open we will have to make some edits.

First, we will need to add the following line to our `Gemfile`. this will import all the proper gems so that our setup matches the one that GitHub is running. I placed mine at the top right after the source.

```ruby
gem "github-pages", group: :jekyll_plugins
```

Next we have to update the package version numbers in our `Gemfile` to match those that GitHub is using. You can see the list [here](https://pages.github.com/versions/).

After you save your `Gemfile` we need to run the update command in the terminal update to get all the proper packages.

```bash
sudo bundle update
```

Finally, we are ready to start up the Jekyll server, this will do two things. First it will watch the file system and every time we save our markdown files it will recompile our site. Second it will run a webserver on port 4000 so that we can preview our work in the browser. I start it with the following command.

```bash
bundle exec jekyll serve
```

Now you can preview your default website by going to [http://localhost:4000](http://localhost:4000).

> NOTE: Changes to `_config.yml` do NOT trigger an auto recompile, you will have to stop and restart the Jekyll server to pickup those changes. Use `ctrl + c` to stop it, and then run the same start command we used a moment ago.

Finally, now you are ready to customize the look and feel of your blog. You can do this from scratch by editing the default theme [Minima](https://github.com/jekyll/minima). Or you can pull a theme from [Jekyll Themes](http://jekyllthemes.org/). I chose the free version of [Hydejack](https://qwtel.com/hydejack/).

## Troubleshooting

- You might see the following error when running some of the `jekyll` commands 'You have already activated public_suffix 3.0.0, but your Gemfile requires public_suffix 2.0.5. Prepending `bundle exec` to your command may solve this.' Following the instructions provided by the error message resolves the issue for me.

- If you get this error 'An error occurred while installing nokogiri (1.8.1), and Bundler cannot continue.' you likely have a missing package, try running this:

```bash
sudo apt-get install zlib1g-dev
```
