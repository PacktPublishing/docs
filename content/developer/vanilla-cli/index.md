---
title: Vanilla CLI
tags:
- Developers
- CLI
- Build Tool
category: developer
menu:
  developer:
    identifier: cli
versioning:
  added: 2.4.2
---

Vanilla provides a [command line tool](https://github.com/vanilla/vanilla-cli) to make various tasks easier for developers working on Vanilla Forums core or addons.

Current functionalities include:

- Building frontend assets (scripts, stylesheets, and images).
- Generating cache files for addons.
- Converting addons' [PluginInfo/ThemeInfo](/developer/addons/plugin-theme-info) to the [addon.json format](/developer/addons/addon-info).
- Validate addons in a Vanilla installation.

## Contents

- [Getting Started](#getting-started)
- [Build Tools](#build-tools)
- [Addon Utilities](#addon-utilities)
- [Common Issues](#common-issues)

## Getting Started

Follow the [setup guide](/developer/vanilla-cli/installation).

## Build Tools

The Vanilla CLI bundles it's own build tool to make starting and mainting your Vanilla Forums addons easier.

- [Build Tool Quickstart Guide](/developer/vanilla-cli/build-quickstart)
- [Build Process - Core](/developer/vanilla-cli/build-process-core)
- [Build Process - 1.0](/developer/vanilla-cli/build-process-v1)
- [Build Process - Legacy](/developer/vanilla-cli/build-process-legacy)
- [How Bundling Works](/developer/vanilla-cli/bundling-process)

Both the core of Vanilla and its many addons often have their own tools to build their frontend dependencies. Normally these tools bundle, concatenate, and/or minify the javascript and styles, compress images and other assets, and may include a CSS authoring tool such as [Sass](http://sass-lang.com/) or [Less](http://lesscss.org/). Many of these build toolchains accomplish the same objective but in different ways.

The Vanilla Build Tool aims to provide a consistant experience to building frontend assets for Vanilla. If you specify a build process in your [addon.json](/developer/addons/addon-info/#build) it will use that process automatically, but falls back to [legacy build process](/developer/vanilla-cli/build-process-legacy) for older projects. This is to try to smooth the edges of working with older addons, but is not as simple as using one of the built in processes.

### Usage
`vanilla build [<options>]`

### Options

#### `--process [process_version]`
Select the build process you wish to use. This will override any other method of settings such the `build.processVersion` in the [addon.json](/developer/addons/addon-info/#build). Current options are [core](/developer/vanilla-cli/build-process-v1), [v1](/developer/vanilla-cli/build-process-v1) and [legacy](/developer/vanilla-cli/build-process-legacy).

#### `--csstool [tool_name]`
Select the CSS preprocessor to use. Current options are `scss` and `less`. The default is `scss`.

#### `--watch`
Run the build process in watch mode. This will listen for changes in your code and recompile the parts that have changed. It spawns a local server meant to hook into the [livereload](http://livereload.com/extensions/) browser extension for [Chrome](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/livereload/), and [Safari](http://livereload.com/extensions/). This is officially supported by the `1.0` build process only. It may also work with the `legacy` build process if the addon supports it.

#### `--verbose`
Log additional output to stdout. This is helpful for finding out what might be wrong in your build process and outlines each step as they occur.

#### `--reset`
This tool has its own javascript dependencies that it relies on to function properly. Some of these are native modules and may require recompilation if your OS or Node.js installation get upgraded. This flags clears all cached modules and reinstall/recompiles them. The tool will attempt to do this automatically if necessary, but this command can be useful for fixing dependency related issues.

## Linting Tools

The Vanilla CLI bundles a linter that enforces Vanilla Forums standards of code quality for SCSS stylesheets and javscripts files. It is built using [ESLint](https://eslint.org/) and [StyleLint](https://stylelint.io/). Options are determined with fallbacks in this order CLI Options/Args > Configuration > Defaults. Configuration can be specific in the [addon.json file](/developer/addons/addon-info#lint).

### Usage
`vanilla build [<options>] [<arguments>]`

### Options

#### `--scripts`

Causes the tool to only run the javascript related parts of the process.

#### `--scripts`

Causes the tool to only run the javascript related parts of the process.

#### `--watch`

Run to the tool in watch mode. Changed files will be linted. Currently newly added files require a re-running the command.

#### `--fix`

Lint the files and attempt to automatically fix certain types of linting errors.

### Arguments

Arguments should be file paths or globs to be linted. The default arguments are `"src/**/*.js" "src/**/*.jsx" "src/**/*.scss"`. `node_modules` and `vendor` directories will be ignored automatically. This allows arbitrary files to be linted.

Be sure to put quotes around glob arguments. If there are no quotes a glob bash/zsh/fish may automatically expand the glob, which will prevent files from being ignored properly.

### Linting Rules
For scripts the CLI bundles the [default ESLint rules](https://eslint.org/docs/rules/). It also bundles its config which can be found [in the Github repo](https://github.com/vanilla/vanilla-cli/blob/master/src/NodeTools/Linter/configs/.eslintrc).

For stylesheets the CLI bundles both [the default Stylelint rules](https://stylelint.io/user-guide/rules/) and the [stylelint-scss rules](https://github.com/kristerkari/stylelint-scss). It bundles a very minimal config which can be find [in the Github repo](https://github.com/vanilla/vanilla-cli/blob/master/src/NodeTools/Linter/configs/.stylelintrc).

## Addon Utilities

The Vanilla CLI offers a few utilities to make managing your addons easier. They only work with addons currently installed in a Vanilla installation and function by using Vanilla's built in addon manager to do the heavy lifting. As a result, these commands require you to point them to the vanilla directory with the `--vanillasrc` parameter.

Alternatively you can set the the environmental variable `VANILLACLI_VANILLA_SRC_DIR` to the installation path.

## `vanilla addon-cache [<options>]`

Reads the vanilla's addon-cache.

### Options

#### `--vanillasrc`

You vanilla installation's source directory.

#### `--regenerate`

Will clear the addon-cache and regenerate it. This is useful for priming the cache without needing to load a page.

## `vanilla addon-doctor [<options>]`

Scans for issues in your addons.

### Options

#### `--vanillasrc`

You vanilla installation's source directory.

## `vanilla addon-json [<options>]`

Convert addons from using `$PluginInfo` / `$ThemeInfo` / `about.php` metadata declaration to the [newer addon.json format](/developer/addons/addon-info). This will convert all addons linked to you vanilla installation.

### Options

#### `--vanillasrc`

You vanilla installation's source directory.

## Common Issues

There are a few common issues that people run into.

### I'm having trouble getting the parameters right.

The CLI tool has some help baked right into the tool! By appending `--help` to any command you can list subcommands or parameters right in your console.

```bash
$ vanilla build --help
$ vanilla addon-json --help
```

### I'm trying to resolve an issue with my build myself but there is not enough output to figure out what's going on.

The CLI tool has a verbose mode which can output additional information to the console. Use it by adding the `--verbose` flag.

```bash
vanilla build --verbose
```

### I'm getting an error message that my Vanilla source directory is missing or incorrect.

In order to function properly the CLI tool needs to know where your Vanilla installation is located on your system. These examples will use `~/workspace/vanilla` as the vanilla directory. The location on your local machine may be different. This can be passed on every command with the `--vanillasrc` parameter.

```bash
vanilla build --vanillasrc=~/workspace/vanilla
```

Note that this is a ***temporary solution***. It will not carry across different shells or reboots. For a more permanent solution you can set an [environmental variable](https://www.cyberciti.biz/faq/set-environment-variable-unix/).

```bash
# For Bash.
# In ~/.bashrc, ~/.profile, or ~/.bash_profile. 
# This will save the variable persistently across all of your sessions.
export VANILLACLI_VANILLA_SRC_DIR=~/workspace/vanilla

# In fish shell.
# This will save the variable persistently across all of your sessions.
set -Ux VANILLACLI_VANILLA_SRC_DIR ~/workspace/vanilla
```

Note that there are *not* quotes around any of the paths.

### I'm having an issue with the CLI's own node_module dependancies.

Try running your command with the `--reinitialize` flag. This will force-reinstall the node_modules required for your particular command.

```bash
vanilla build --reinitialize
```

### I'm getting an error related to composer, autoloader, or some PHP function not being available.

This applies only to manual installations of `vanilla-cli`. Navigate to your `vanilla-cli` directory and run `composer install`.
