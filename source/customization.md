---
title: Customization
date: 2017-07-11 13:41:03
---
# Customization

## <a id="permalinks"></a> Permalinks

You can specify the permalinks for your site in `_config.yml`

The default value is: 

`categories/year/month/day/title`

Supported variables are:

Variable | Description
------------ | -------------
categories | Categories extracted from the post __Front-matter__
year | Year, extracted from post publication date (4-digits)
month | Month, extracted from post publication date (2-digits)
day | Day of the month, extracted from post publication date (2-digits)
title | Slugified post title.

## <a id="themes"></a> Themes

The active theme is defined by the `theme` variable on your site configuration file `_config.yml`.

The default value is `glayu-times`, it enables [The Glayu Times](https://github.com/pmartinezalvarez/glayu-times-theme) theme, downloaded to the `themes/glayu-times` folder after running `init`.

If the variable `theme_uri` is defined, the `init` command will try to download the selected theme from the specified uri.

To create a new theme, create a directory under the `themes` folder. The directory name is the value you will have to provide on `_config.yml` to activate your theme.

A theme should have the following structure:

```
.
├── _layouts
├── _partials
└── assets
```

### _layouts

This folder contains the theme’s template files, which define the appearance of your website. Glayu uses the [Embedded Elixir (EEx)](https://hexdocs.pm/eex/EEx.html) templates engine.

### _partials

This folder contains components that are shared between your templates.

### assets

Compiled assets, this directory will be copied to the public site directory on each build.

## <a id="templates"></a> Templates

Templates are stored under the `_layouts` folder and define the presentation of your site. The table below shows the corresponding template for every available page.

Template | Page
------------ | -------------
`layout.eex` | Global layout common to all pages
`home.eex` | site home
`post.eex` | posts
`page.eex` | pages
`category.eex` | category home

### Global Layout

The global layout provides a structure shared by all pages. It should contain a `<%= @inner %>` directive that will be replaced with the content of the corresponding template, depending on the page type.

#### Example

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= @page.title%></title>
    <!-- Style sheets -->
  </head>
  <body>
    <%= @inner %>
    <!-- Scripts -->
  </body>
</html>
```

### Partials

Partials are a tool to share components between templates. Typical use cases are page sections like the header, footer or navbars.

All `.eex` files placed under the `_partials` folder will be compiled and can be required inside any template using the directive:

```html
<%= partial "<partialname>" %>
```

Before using the `partial` directive you have to declare that you want tu use the `Glayu.EEx` module:

```html
<% use Glayu.EEx %>
```

#### Example

`_partials/header.eex`

```html
<h1 id="header"><%= @page.title%></h1>
```

`_layouts/home.eex`

```html
<% use Glayu.EEx %>
<%= partial "header" %>
<div>
<!-- page content -->
</div>
```

Will be handled as:

```html
<h1 id="header"><%= @page.title%></h1>
<div>
<!-- page content -->
</div>
```

## <a id="variables"></a> Variables

### Site Variables

Variable | Description | Type
------------ | ------------- | -------------
`@site.title` | Site title | String

### Page Common Variables

Variable | Description | Type
------------ | ------------- | -------------
`@page.title` | Page Title | `String`
`@page.layout` | Page layout `:post`, `:page`, `:category` or `:home` | `atom`
`@page.type` | Page type `:draft`, `:post`, `:page`, `:category` or `:home` | `atom`
`@page.path` | Page relative URL path | `String`

All extra __Front-matter__ variables will be added to the `@page` scope.

### Post and Page Common Variables

Variable | Description | Type
------------ | ------------- | -------------
`@page.date` | Page publication date | [`DateTime`](https://hexdocs.pm/elixir/DateTime.html)
`@page.source` | Source markdown file path | `String`
`@page.raw` | Content as Markdown | `String`
`@page.content` | Content as HTML | `String`

### Post Specific Variables

Variable | Description | Type
------------ | ------------- | -------------
`@page.categories` | Post categories. It is a `List` where each item is a `Map` including following fields: `keys`, `name`, `path`. The `keys` identify the category and are required to retrieve extra information like the category latest posts. `name` is the category name. And `path` is the relative URL to the category home page. | `List`
`@page.summary` | Post summary (HTML) | `String`

Optional Variables

Variable | Description | Type
------------ | ------------- | -------------
`@page.tags` | Post tags list.  | `List`
`author` | Post author | `String`
`featured_image` | Featured image | `String`
`score` | Page relevance score | `String`

### Category Page Specific Variables

Variable | Description | Type
------------ | ------------- | -------------
`@page.category` |  Current category | `Map`
`@page.category.keys` | Category Keys | `List`
`@page.category.name` | Category Name | `String`
`@page.category.path` | Relative URL path, the same value as `@page.path` | `String`
`@page.category.parent` | Parent category on the site hierarchy, includes the fields: `keys`, `name` and `path` | `Map`


## <a id="helpers"></a> Helpers

The `Glayu.EEx` module includes a set of helpers that can be invoked in your templates after include the `<% use Glayu.EEx %>` directive.

### Categories

```
categories()
```

Returns the list of first level categories of the site. Each category is a `Map` containing following fields:

Field | Description | Type
------------ | ------------- | -------------
`keys` | Category keys | `List`
`name` | Category name | `String`
`path` | Category home page relative URL | `String`

### Subcategories

```elixir
subcategories(<category_keys>)
```

return the list of subcategories of a given category.

#### Arguments

Field | Description
------------ | -------------
`keys` | Parent category keys 

#### Examples

Rendering the sub-categories menu on a category page.

```html
<nav id="mini-navigation">
  <ul class="nav navbar-nav">
  <%= for subcategory <- subcategories(@page.category.keys) do %>
  <li><a href="<%= subcategory.path %>"><%= subcategory.name %></a></li>
  <% end %>
  </ul>
</nav>
```

### Latest Posts

```elixir
posts([limit: <num_posts>, sort_fn: <sort_fn>])
```

List the latest site posts.

#### Arguments

Argument | Description
------------ | -------------
`<num_posts>` | Number of post to be retrieved (max. 100).
`<sort_fn>` | Sorting function (applied over the last 100)

#### Examples

Displaying most relevant post of the site.

```html
<%= for post <- posts([limit: 10, sort_fn: &(&1.score > &2.score)]) do %>
<div class="article">
  <%= if post[:featured_image] do %>
  <img src="<%= post.featured_image %>" alt="featured image">
  <% end %>
  <h2><a href="<%= post.path %>"><%= post.title %></a></h2>
  <div><%= post.summary %></div>
</div>
<% end %>
```
### Latest Category Posts

```elixir
posts(<category_keys>, [limit: <num_posts>, sort_fn: <sort_fn>])
```

works similar but limiting the returned post to a specific category.

### Date Formatter

```elixir
Date.format(<date>, <strftime_format>)
```

Formats an article date.

#### Arguments

Argument | Description
------------ | -------------
`<date>` | Date to be formated
`<strftime>` | [strftime](http://strftime.org/) compatible expression, that defines the date formal.

#### Examples

```html
<%= Date.format(post.date, "%Y-%m-%d %H:%M") %>
```

Will render a date formated like: `2017-05-18 06:45:22`

### Current Date

```elixir
Date.now(<strftime_format>)
```

Formats current date and time.
