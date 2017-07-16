---
title: Commands
date: 2017-07-11 13:40:40
---
# Commands

## <a id="init"></a> init

```console
glayu init [folder]
```

Initializes the website.

If no `folder` is provided, the website will be configured in the current directory.

If the destination folder exists and contains a `_config.yml` file, this configuration file will be used to initialize the site.

#### Arguments
Argument | Description
------------ | -------------
`folder` | Website destination folder. If the destination folder doesn't exists the full path to it will be created.

#### Examples

```console
$ mkdir my-glayu-site
$ cd my-glayu-site
$ glayu init
```
or

```console
$ glayu init ./my-glayu-site
```

## <a id="new"></a> new

```console
glayu new [layout] <title>
```

Creates a new post or page. 

All new posts are considered drafts and will placed under the `source/_drafts` directory, this directory will be skipped during a site build. When a post is ready it can be published using the `publish` command.

All pages are placed under the `source` directory, and are rendered during a site build.

#### Arguments
Argument | Description
------------ | -------------
`[layout]` | __post__ or __page__. If no layout is provided, Glayu will use the post layout.
`<title>` | Article title. If the title contains spaces, surround it with quotation marks.

#### Examples

```console
$ glayu new "My First Glayu Post"
```

## <a id="publish"></a> publish

```console
glayu publish <filename>
```

Publishes a draft.

The markdown file will be moved from the `source/_drafts` directory to a directory under `source/_posts` following the permalik definition provided in `_config.yml`.

#### Arguments
Argument | Description
------------ | -------------
`<filename>` | Markdown source file. The file name or the file path can be provided.

#### Examples

```console
$ glayu publish my-first-glayu-post.md
```

or

```console
glayu publish {site_dir}/source/_drafts/my-first-glayu-post.md
```

## <a id="build"></a> build

```console
glayu build [-chp] [regex]
```

Generates the static files.

#### Arguments

Argument | Description
------------ | -------------
`[regex]` | If provided, only the pages under the directories matching the regex will be generated. The regular expression is based on PCRE (Perl Compatible Regular Expressions)

#### Options

If one of the `-chp` options is provided the building process will consider only the resource types provided on the options.

Option | Description
------------ | -------------
`[-c, --categories]` | Adds category pages to the building pipeline.
`[-h, --home]` | Adds home page to the building pipeline.
`[-p, --pages]` | Adds post and pages to the building pipeline.


#### Examples

```console
$ glayu build 
```
generates the full site

```console
$ glayu build _posts/business/.*2017/.*
```

generates all 2017 business pages

## <a id="help"></a> help

```console
glayu help [command]
```

Displays help.

If `command` is not specified, all of the commands supported by Glayu are listed.

#### Arguments


 Argument    | Description
------------ | -------------
`[command]`  | Displays help information on that command.
