---
title: Basic Usage
date: 2017-07-11 13:40:33
layout: lateral-menu-page
---

# Basic Usage

## <a id="site-setup"></a> Site Setup

After installing Glayu, run the `init` command to generate a Glayu site in the target `<folder>`.

```console
$ glayu init <folder>
```

You project folder will look like:

```
.
├── _config.yml
├── public
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

### _config.yml

Site configuration file.

```yml
# Glayu Configuration

# Site
title: A brand new static site              # Site Title

# URL
permalink: categories/year/month/day/title  # Permalink format

# Directories
source_dir: source                          # Source folder
public_dir: public                          # Destination folder

# Theme
theme: glayu-times                          # Selected Theme under themes dir
theme_uri: https://github.com/pmartinezalvarez/glayu-times-theme/archive/master.zip
```

### source

The source folder. This is were your site content resides. All Markdown files under the `source` folder will be processed and the generated HTML put into the `public` folder, except the files inside the `_drafts` folder, that will be ignored.

### themes

Themes folder. Glayu will generate the final site combining the site contents with the theme selected on the configuration file.

The default theme is [The Glayu Times](https://github.com/pmartinezalvarez/glayu-times-theme)

## <a id="writing"></a> Writing

You can create a new post or page using the `new` command:

```console
glayu new [layout] <title>
```

`new` command supported layouts are __post__ and __page__

### Drafts and Posts

To create your first post run:

```console
$ glayu new post "My First Glayu Post"
```

It will generate the Markdown file `source/_drafts/my-first-glayu-post.md`. Glayu slugifies the post title and uses it as filename.

All new posts are _drafts_, and will be saved into the `source/_drafts` directory. This directory will be skipped during site builds.

#### Front-matter

The new Markdown file will look like:

```yml
---
title: My First Glayu Post
date: 2017-05-18 06:45:22
author:
featured_image:
score: 10
summary:
categories:
- Software
- Static Sites
tags:
- Glayu
- Elixir
---
A new Glayu post
```

At the beginning of the file, delimited by three dashes, is the __Front-matter__. It is a block of YAML used to configure your article variables. Just below is the post content.

Variable          | Description                   | Default
------------      | -------------                 | -------------
__`title*`__      | Article Title                 |
__`date*`__       | Publication Date              | Creation Date
__`categories*`__ | Article Categories (top-down) | Software, Static Sites (demo)
`author`          | Article Author                |
`featured_image`  | Article main image            |
`score`           | Article priority, used by themes to render top stories (the higher the better) | 10
`summary `        | Article summary, if not present the first content paragraph will be used |
`tags`            | Article Tags                  | Glayu, Elixir (demo)


__`*`__ Required variables.

All new variables added to the Front-matter will be processed and includes on the page scope.

#### Publishing a Draft

Once your post is ready, you can move it to the `source/_posts` folder using the `publish` command:

```console
$ glayu publish my-first-glayu-post.md
```

The Markdown file will be moved from `source/_drafts/my-first-glayu-post.md` to `source/_posts/software/static-sites/2017/05/18/my-first-glayu-post.md`. The destination path will be calculated using the `permalink` value defined in `_config.yml`: 

```yml
permalink: categories/year/month/day/title
```

The Markdown will be rendered on next site build.

### Pages

All pages are generated under the `source` directory and will be processed on next build.

By default, a page __Front-matter__ includes:


Variable     | Description      | Default
------------ | -------------    | -------------
__`title*`__ | Article Title    |
__`date*`__  | Publication Date | Creation Date 


__`*`__ Required variables.

## <a id="site-generation"></a> Site Generation

You can generate your site using the `build` command:

```console
$ glayu build
```

Will generate the full site.

If you want to render only part of your posts you can pass a regular expression to the `build` command. Supported regular expressions are based on PCRE (Perl Compatible Regular Expressions):

```console
$ glayu build _posts/software/.*2017/
```

will render the posts under `software` published on 2017.

## <a id="preview-server"></a> Preview Server

Glayu comes with a preview server to monitor your site changes during the development.

To start the server on the default port [http://localhost:4000/](http://localhost:4000/) run:

```console
$ glayu serve
```

### Pre-requisites

The Glayu preview server uses the [file_system](https://github.com/falood/file_system) module that depends on:

* Mac fsevent
* Linux and FreeBSD inotify
* Windows inotify-win

It will look for the required backend binary on you system `PATH`:

* On windows download and add [inotifywait.exe](https://github.com/falood/file_system/blob/master/priv/inotifywait.exe) to your `PATH`
* On Linux and FreeBSD install the `inotify-tools` package.
* On Mac you can generate the `mac_listener` from the [file_system](https://github.com/falood/file_system) source using the `mix` command. After building the project you will find the binary under `./priv/mac_listener`.

### Preview URIs

Your can preview your drafts, posts and pages by following these examples criteria:

Type | Source | Preview url
------------ | ------------ | -------------
Draft | `./source/_drafts/my-first-glayu-post.md` | `http://localhost:4000/_drafts/my-first-glayu-post.md`
Post | `./source/_posts/software/static-sites/2017/07/11/my-first-glayu-article.md` | `http://localhost:4000/software/static-sites/2017/07/11/my-first-glayu-article.html` and `http://localhost:4000/_posts/software/static-sites/2017/07/11/my-first-glayu-article.html`
Page | `./source/my-first-glayu-page.md` | `http://localhost:4000/my-first-glayu-page.html`


