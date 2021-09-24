---
toc: true
layout: post
comments: true
image: images/kaggle-logo.png
permalink: /kaggle-setup/
description: A guide to setup Kaggle API on Linux/Mac.
categories: [kaggle, markdown]
title: Setting up Kaggle on Linux/Mac
---

Most of latest data science innovations happen at [Kaggle](https://www.kaggle.com).
Kaggle hosts, in addtion to competitions, a large collection of datasets from various
fields. The easiest way to interact with Kaggle is through its public API via command-line
tool(CLI). Setting it up outside of Kaggle kernels is one of first tasks. In this post, I
will guide you through that process.

> **Pre-requisite**: Python3(>3.6) and latest pip installed.

## Installation

```shell
pip install --user kaggle
```

> **Tip**: Install `kaggle` package inside your conda ML development environment rather than outside of
> it or in base env.

> **Warning**: Don't do `sudo pip install kaggle` as it would require admin privileges for
> every run.

## Download API token

1. Create/login into your kaggle account.
1. From the site header, click on your user profile picture and select Account. You will
   be land on your profile with account tab active.
1. Scroll down to API section. Click `Create New API Token`. A `json` file will be
   downloaded your default download directory.

## Move .json file to the correct location

1. Move it to `.kaggle` in the home directory. Create if absent.
```shell
cd
mkdir ~/.kaggle
mv <location>/kaggle.json ~/.kaggle/kaggle.json
```
1. For your security, ensure that other users of your computer do not have read access to your credentials.
On Unix-based systems you can do this with the following command:
```shell
chmod 600 ~/.kaggle/kaggle.json
```
1. Restart the terminal and navigate to the env where kaggle package is installed if necessary.

## Check if it is properly installed

1. Run:
```shell
$python
>>>import kaggle
```
Importing kaggle shouldn't return an error. If there is error, check whether you're in the
right env where kaggle is installed.

If no error, exit the shell and type the following command in the terminal.

```shell
kaggle competitions list
```

If installed properly, the command will list all the entered competitions.
1. If not, the binary path may be incorrect. Usually it is installed in `~/.local/bin`
Try using
```shell
~/.local/bin/kaggle competitions list
```
1. If the above command works, export that binary path to the shell environment(*bashrc*)
   so that you might use just `kaggle` next time.

## API usage

It is time to use the Kaggle API. For example, to see what dataset command offers, in the CLI enter
```shell
kaggle dataset --help
```

> **Tip**: Remember to comply with competition's terms and conditions before downloading
> the dataset. You will get an error `forbidden` if you try to download before agreeing.

For more info on the API, Kaggle's [github](https://github.com/Kaggle/kaggle-api#commands) page is an excellent resource.

