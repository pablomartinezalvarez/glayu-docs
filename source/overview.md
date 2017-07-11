---
title: Overview
date: 2017-07-11 12:56:03
---
## What is Glayu?
Glayu is a static site generator focused on mid-sized sites, that generate content frequently and have to deal with multiple categories, like magazines and newspapers.

## What makes Glayu different?

What makes Glayu different from other static site generators is the way it structures the source folder: when you publish an article using Glayu the .md file is placed inside a subfolder of the `source` directory following the permalink definition.

With the default permalink definition `categories/year/month/day/title`  your `source` and `public` directories will look like:

```
TODO Image
```
   
The motivations of this `source` folder organization is to split the site content in smaller units that can be managed independently. Glayu takes advance of this folder organization to enable the concurrent and partial generation of the site. In the previous example a complete build of the site will handle the `/source/_posts/world/2017/07/11` and `/source/_posts/us/2017/11`  folders concurrently. Glayu enables the partial generation of the site by using regular expressions, so if you will be interested in only generate the `world` articles you could use the `build` command like this:
   
```console
$ glayu build _posts/world*
```
   


