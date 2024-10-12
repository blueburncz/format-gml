# format-gml

> Tiny tool and a Git hook for automatic formatting of GML code

[![License](https://img.shields.io/github/license/blueburncz/format-gml)](LICENSE)
[![Discord](https://img.shields.io/discord/298884075585011713?label=Discord)](https://discord.gg/ep2BGPm)

## Table of Contents

* [About](#about)
* [How to set up](#how-to-set-up)
  * [As a repo owner](#as-a-repo-owner)
  * [As a contributor](#as-a-contributor)
* [Common issues](#common-issues)

## About

format-gml is a simple Python script that uses [js-beautify](https://github.com/beautifier/js-beautify) to format GML files, designed primarily to ensure that all contributors to an open-source GameMaker project use the same code formatting style. This is achieved with a pre-commit Git hook, which checks whether all staged GML files follow the style defined in a file `.jsbeautifyrc` placed in the root of the repo. If a staged GML file doesn't follow the formatting style, it's not allowed to be committed before it's fixed. This can be done simply by running the tool.

## How to set up

### As a repo owner
First, you need to download a release of format-gml and extract it into the root of your repo. Don't forget to commit all the extracted files! 🙂

Next, install [Python 3](https://www.python.org/downloads/) and the required packages by running the following command:

```sh
pip3 install -r requirements.txt
```

You may want to customize the `.jsbeautifyrc` file to match your preferred code formatting style. Our default setup follows the [Allman style](https://en.wikipedia.org/wiki/Indentation_style#Allman_style), with tabs for indentation (size 4) and a maximum line length of 120 characters. You can have a look here for all available options: <https://github.com/beautifier/js-beautify?tab=readme-ov-file#options>.

To format all GML files present in your repo with the configure style, run the following command and commit the changes.

```sh
python3 format-gml.py --all
```

Afterwards, set up the hook by running:

```gml
git config core.hooksPath hooks
```

And that's it! From now on, when you try to commit changes to GML files within the repo, the hook won't allow you to do so if they're not properly formatted. To fix their formatting, simply run `format-gml.py` with out any arguments like so:

```sh
python3 format-gml.py
```

### As a contributor

1. Install [Python 3](https://www.python.org/downloads/) and the required packages by running the following command from the root of the repo:

```sh
pip3 install -r requirements.txt
```

2. Set up the hook by running:

```gml
git config core.hooksPath hooks
```

3. When you're not allowed to commit a file because of its formatting, run the following command and stage the changes:

```sh
python3 format-gml.py
```

## Common issues

Since js-beautify is a JavaScript formatter, it doesn't work properly on GameMaker Language in all cases.

1. Strings `@""` and `$""`

Handled automatically inside of `format-gml.py` by removing all whitespace between characters `@`/`$` and `"`.

2. Data structure accessors

Handled automatically inside of `format-gml.py` by removing all whitespace between characters `[` and `@`/`|`/`#`/`?`/`$`.

3. `#macro`s

Currently the only fix for those is to surround them with `/* beautify ignore:start */` and `/* beautify ignore:end */` like so:

```gml
/* beautify ignore:start */
#macro MY_FANCY_MACRO \
    do
    { \
        some_stuff_here(); \
    } \
    until (1)
/* beautify ignore:end */
```

If you find more, please let us know via an issue or contact us on our Discord server (link at the top).
