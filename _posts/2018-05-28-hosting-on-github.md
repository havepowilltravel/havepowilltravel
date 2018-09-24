---
layout: post
title: "Spinning Up On Github -- A Cookbook"
date: 2018-05-28 08:45:00 -0400
categories: jekyll github
---

OK, so this is a sweet awesome cookbook ... yeah, more cookbook than recipe, for bringing up a blog on [Github Pages][Pages].  It's not for total noobs, 'cause, yeah, we're gonna do stuff with the deaded command line, and the post doesn't deal with the pecularities of using *nix-native tools in the Windows environment.

# So Ya Wanna Host A Blog On GitHub?

Well, here's your chance!  A step-by-step recapitualtion of what it takes.  I'm wondering if "Spinning Up" is the best title for this; after all, SSD's don't spin.

Note:  This assumes Jekyll (and Ruby) are installed and functional on your local workstation.  Tested with Ruby 2.4.1 and Jekyll 3.7.3; YMMV. For basic Jekyll information, well, as Tech Support says, "[RTFM][jekyll-docs]".

# Getting Started

Create your Jekyll site locally.  Change your working directory to where ever you'd like the blog's directory tree to live, then create a new Jekyll project.  You may call it whatever you'd like, within the naming restrictions of your environment.  I'm lazy, so it's going to be `blog.`

```
> cd blogs-folder
> jekyll new blog
```

Having a somewhat orderly sense of order and organization, I prefer to keep my WIP in a drafts directory.  By convetion this directory is called `_drafts` and rests in the top level of the blog's directory structure.  Earlier versions of Jekyll didn't understand drafts, everything had to be in `_posts` with proper naming syntax, but with this version (I don't know when it was introduced), the `_drafts` folder is understood.  Again, [RTFM.](https://jekyllrb.com/docs/drafts)  So go ahead and create the `_drafts` directory.

```
> cd blog
> mkdir _drafts
```

A common error by non-techie types is to not use the correct file extension on the post's files.  If you're writing Markdown (most common but not the only possibility), it's `.markdown` or `.md`.  Nothing else will be understood by the parsers, and the files will not be included in the built site.  Daring Fireball has posted a very nice Mardown [cheatsheet.](https://daringfireball.net/projects/markdown/syntax)

Since this post is about getting a blog up and running on Github, I'm going to ignore (for the time being; check back for a future post) all the stuff about setting up your blog environment: the title, a page footer, an about post, alternative themes, and so forth.  These details are ably covered in [TFM][jekyll-docs].

Setting up the blog locally is not strictly necessary.  It can be done entirely in the Github environment.  I find that it's just a lot more convenient to work locally and then upload relatively finished work to the public site.

# Local Jekyll

With Github pages, you benefit from using the gihub-pages gem (more below), which doen't require a local install of Jekyll. The advantage of having Jekyll installed locally is that you have the `jekyll new` command, which will build the basic scaffold of the web site for you.  And if you want to deploy a web site somewhere other than Github, you'll want Jekyll.

# Speak Your Mind

Write a post or two, or three.  And I'll write some more about shell scripts to move a post from `_drafts` to `_posts`.

# Publishing on Github

# Why Github?

First, the price is right. Second, it's Jekyll-friendly; not a lot of pushing on a rope to get things working.  It's all pretty simple, really.

# Why Not Github?

Well, it's not Wordpress.  For a real discussion of this, see the [About][about] post.

Digital Ocean costs money.  And you need to a bunch of pushing.  Heroku has a free tier, great support for Ruby, and they have become more amenable to Jekyll.  But their free tier goes to sleep, so a reader will have to wait for the dyno to come back to life if it's been idle for an hour or so.

And for the Microsoft haters out there (I am mos def not a fanboi and think that Maddog's vision of total world domination by open source is a wonderful antidote for WinTel hegemony), if Redmond's new owneship of Github becomes onerous, having your blog site in a local repo frees you to take your posts and play elsewhere.  In the meantime, the price is right.

Another consideration is around the _drafts folder, if you use one.  It's certainly enticing to version control your drafts.  I tend to have quite a few half-baked thoughts in draft form and it would really suck to loose them in the event of an accident.  But do they need versioning?  Will you be wanting to roll a blog post back to a previous incarnation?  Or will a good backup and recovery (it ain't a backup if you can't recover it: Practice!) strategy be sufficient to protect from accident?  You need to back up the git repo anyway, unless you're considering the Github version to be that.  

Github also imposes some resource limits on user pages.  Since these things tend to change over time, it's best to see what is currently imposed by reviewing the Github pages [documentation][usage-limits].

# OK, So How?

1. Put your blog site into git.
2. Set up Github.
3. Push the site repo's master branch.
4. Voila!

It sounds so so simple.

Now the dirty details.

### To Local Repo Or To Not Local

Github's user pages (the path of least resistance) build from the master branch of the git repo you create on github.io.  See various other web posts for details around this.  So why would you want another, local repo?

1.  For testing the layout of the posts.
2.  For trouble-shooting Jekyll build problems.
3.  If you want to experiment with other themes and assorted plug-ins and things just get really fubar, you can `rm -R` on the directory tree, pull the Github repo and have a clean restart.
4.  Github imposes a soft limit of 10 site builds per hour, which may not be enough early in your Markdown/Jekyll/Jekyll theme career.  You may be doing far more than that tweaking things like post formatting.  Building, reviewing and refining your site locally before pushing doesn't challenge that limit.

# Update the Gemfile

The github-pages gem deals with assorted dependencies from Github.  It also provides a degree of management of the environemnt, and obviates the need for a local install of Jekyll.  The Jekyll build and server commands still work; you just don't need to separately require Jekyll in the Gemfile.

To see a full list of the gems github-pages will drag in, add it to the Gemfile, run `bundle install` and then `bundle exec github-pages versions`.  You might be surprised.  Even with a pretty minimal Gemfile (mostly generated by `jekyll new`):

    source "https://rubygems.org"

    # Hello! This is where you manage which Jekyll version is used to run.
    # When you want to use a different version, change it below, save the
    # file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
    #
    #     bundle exec jekyll serve
    #
    # This will help ensure the proper Jekyll version is running.
    # Happy Jekylling!
    # gem "jekyll", "~> 3.8.2"  -- not needed with github

    # This is the default theme for new Jekyll sites. You may change this to anything you like.
    gem "minima", "~> 2.0"

    # If you want to use GitHub Pages, remove the "gem "jekyll"" above and
    # uncomment the line below. To upgrade, run `bundle update github-pages`.
    # gem "github-pages", group: :jekyll_plugins

    # If you have any plugins, put them here!
    group :jekyll_plugins do
       gem "jekyll-feed", "~> 0.6"
       gem "github-pages"
    end


# Put the Blog Site Into Git

Github builds the site every time it's pushed, so you don't want the _site directory tree in the repo.  After all, you can always rebuild it, right?  There are a couple other directory trees you don't need to include in the repo, as well:

    _site/
    .sass-cache/
    .jekyll-cache/
    .jekyll-metadata

You may or may not want your _drafts directory under version control; that's a personal decision.  Without doing some fancy git footwork, including drafts in the repo will also push them to Github.  If you do not want _drafts in the repo, add an entry to .gitignore.  

When you initialize a new git repo for the blog, edit .gitignore to include those four (or five) lines.  As Jekyll evolves, it gets smarter, and the `jekyll new` command  scaffolds a .gitignore file for you.

Github's user pages (the free ones) only build from the repo's `master` branch, so that's where you need to add your source files to your local repo.

To add your files to your repo, in the root directory of your web site, excute the following commands:

    git add .
    git commit -m "a comment describing what I am committing"

The `add .` command adds any new or changed files to the repo's "tracked files" list.  You may certainly continue to edit a file after adding it; the `add` stays in effect until you issue a `commit.`  You may also add individual files: `add path/filename` where path is the relative path from the current working directory.  Learn `man git` and `git --help` -- they are your friends.

Yes, after you commit, you will need to issue another add.


# Configure Github

There is a very good review of the steps needed in the [Github help documents][github-pages], so my running commentary will mostly deal with gotchas.

Jonathan McGlone currently with the University of Michigan library system has written a [really great guide][mcglone-guide] to setting up a personal website on Github.  He includes a section on using Jekyll.  But he does all his work on Github, not using a local repo.  Nonetheless, it's a terriffic recapitulation.

The first few things to do are:

1. Log into github.com.  Create an account if you don't already have one.
2. Create the new git repo that will drive the blog.  Name it `username.github.io` and make sure it's public.  Yeah, it uses your usename as the title, so maybe picking something other than "PurpleSpacePoodle" might be a good idea.  Don't worry about tying the Github url (above) to a custom subdomain like `blog.myname.me` at this point. 


Github has an excellent [summary][add-exisiting-repo] on adding an existing repo to Github.  The individual commands to be executed locally are well-annotated.  In particular, look at steps 1, 7 and 8.  And read about "The site doesn't publish", below.

# Add Github to Your Local Repository as a Remote

`git remote add origin https://github.com/<your-username>/<your-blog-repo-name>`

And then moving your site to Giyhub is as simple as

`git push -u origin master`

Well, maybe not.  If you get a message to the effect
```
To https://github.com/havepowilltravel/havepowilltravel
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/havepowilltravel/havepowilltravel'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
what has happened is that the remote (Github) repository has changes that are not present in your local repository.  How can this happen, if you;re the only person working in the repository?  Well, perhaps, in order to clean up a bunch of 'stuff,' you moved the code for the site to a new directory tree and created a new repository.

If you're sure that your local repository is the ultimate authority, add the `--force` option to the push:

`git push --force -u origin master`

and all should be fine.

After the push finishes, point your web browser at your site: *<your-username>.github.io/<your-blog-repo-name>* and check out your handywork.

# Troubleshooting a Push

The stuff they didn't tell you in college ....

#### The site doesn't publish.

First, look at the repo's settings -- the Settings tab of the repository page.  Almost at the bottom is a section titled titled "GitHub Pages."  When a repo is first created on GitHub, Pages are disabled.  Change the Source to Master Branch and save the changes.  When the Settings page refreshes, it should now say something like "Your site is ready to be published as https://havepowilltravel.github.io/havepowilltravel/."

Having the Pages disabled will prevent the site from publishing.

#### _config.yml

The baseurl and url settings in _config.yml have to be correct, or the site will not display correctly.

1.  The protocol needs to be https:// in the url, not http://.
2.  For GitHub Pages, the baseurl is the name of the repo, so in this instance it's "/havepowilltravel".
3.  The url needs to be "https://yourname.github.io" unless you're using a custom url, in which case read the Github documentation.

David Jacquel has a good discussion of [this][localhost] on Stackoverflow.  But, as noted below, you can just add `--baseurl ""` to the local `jekyll serve` command.

This also affects the application of the Jekyll theme.

By default, the excludes at the bottom of the config file are commented out.  You probably want to uncomment them.

#### Links to static content don't work

Such as about.md.

The _config.yml file needs some changes from the defaults, if you created the site with `jekyll new.`

Note that after setting the baseurl for GitHub, trying to serve your site locally for testing purposes will no longer work.  Why?  the baseurl parmeter 
should be "".  So do we keep bouncing back and forth in _config.yml?  No, add the `--baseurl ""` to your local `jekyll serve` command:

`bundle exec jekyll server --baseurl ""`

More troubleshooting tips are at [Troubleshooting GitHub Pages builds.](https://help.github.com/articles/troubleshooting-github-pages-builds)


[Pages]: https://pages.github.com
[jekyll-docs]: https://jekyllrb.com/docs/home
[github-pages]: https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/
[about]: /about
[mcglone-guide]: http://jmcglone.com/guides/github-pages/
[usage-limits]: https://help.github.com/articles/what-is-github-pges/#usage-limits
[add-existing-repo]: https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/
[localhost]: https://stackoverflow.com/questions/27386169/change-site-url-to-localhost-during-jekyll-local-development
