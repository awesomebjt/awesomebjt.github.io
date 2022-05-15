---
title: "Using Fedora Toolbox to Work with Jekyll"
layout: post
---

The [toolbox](https://docs.fedoraproject.org/en-US/fedora-silverblue/toolbox/) utility provided by Fedora doesn't necessarily make setting up a development environment faster or easier, but it does simplify your task if you don't want your development stacks mingled in with your desktop operating system. Almost every software development stack wants to make its special modifications to your PATH and other environment variables. Before long, especially if you like to dabble in as many things as I do, this creates a lot of clutter that can be hard to clean up, and a horde of virtual machines doesn't feel much like a solution either. What Toolbox does is create containers that are tightly coupled to the host sytem, having access to various ports and volumes, but isolating everything that you're installing and executing inside a container. This gives you some freedom to install a broad range of packages, without all these dependencies turning into something your base operating system has to juggle.

In my case, on Fedora 36, I can check this blog out and start working on it with the following commands.

First I initialize a toolbox if one isn't already available. I don't give it any extra arguments, so it defaults to mimicking my host Linux OS.
```
$ toolbox create
```
Once there is a toolbox I can do this so that my terminal is working from inside the container.
```
$ toolbox enter
```
You'll see a purple dot to the left of your prompt now. This indicates that your commands will now be executed inside the container made by Toolbox. So let's get started with our Jekyll environment. First install some prerequisites. You will require some tools for compiling gems.

```
$ sudo dnf group install "Development Tools"
$ sudo dnf install g++ ruby-devel
```

Then go ahead and install jekyll from the Fedora repositories. This will automatically include ruby and other dependencies.
```
$ sudo dnf install jekyll
```

Now you are free to start a Jekyll project, move into its directory and let bundler install all the necessary gems.
```
$ jekyll new demosite && cd demosite && bundle install
```

This should now work:
```
$ bundle exec jekyll serve
```

If you're working with a Github pages site and this doesn't just work, if you get an error like `cannot load such file -- webrick (LoadError)` just add this to your Gemfile:
```
gem "webrick"
```
And use `bundle install` again.

You how have a functioning Jekyll environment conveniently isolated in a container!