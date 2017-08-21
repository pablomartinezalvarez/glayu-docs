---
title: Installation
date: 2017-07-11 13:40:18
layout: lateral-menu-page
---
# Installation

## <a id="escript"></a> Escript

Glayu is distributed as an Escript executable, before running it, you need [Erlang](http://www.erlang.org/) installed on your computer. You can find the latest Erlang releases under the [Erlang downloads page](http://www.erlang.org/downloads).

Once you have installed Erlang. Download the latest [glayu binary](https://github.com/pmartinezalvarez/glayu/raw/master/apps/glayu/glayu) from the releases section and add it to your PATH. Now you can invoke the `glayu` command.

## <a id="build-from-source"></a> Build From Source

Before building Glayu you need [Elixir](http://elixir-lang.org/) installed on your computer. A detailed installation guide is available on the [Elixir web page](http://elixir-lang.org/install.html).

Once you have installed Elixir:

Clone the Glayu repo: 

```console
$ git clone https://github.com/pmartinezalvarez/glayu.git`
```

Get the project dependencies: 

```console
$ mix deps.get
```

Generate the glayu Escript executable from the `./apps/glayu` application: 

```console
$ cd ./apps/glayu
$ mix escript.build
```

Under the glayu root directory you will find the `glayu` binary file.

You can add the binary to your mix escripts directory using:

```console
$ mix escript.install
```
