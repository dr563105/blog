---
toc: true
layout: post
image: images/neovim-logo.png 
comments: true
permalink: /skim-vimtex-setup/
description: A short post to setup Skim pdf reader with Vimtex plugin in Mac OS.  
categories: [skim, vimtex, neovim, vim, mac]
title: Setup Skim PDF reader with VimTeX in Mac OS
---
[VimTeX](https://github.com/lervag/vimtex) plugin written by Karl Yngve Lervåg is one of the goto plugins to manage
LaTeX files with Vim/Neovim text editors. VimTeX allows integration with several PDF
viewers. In Mac OS, [Skim](https://skim-app.sourceforge.io) and [Zathura](https://pwmt.org/projects/zathura/)
PDF readers allows easy integration with LaTeX. Since Zathura's installation in Mac OS involves more steps, 
we will be using Skim for this post. 

{% include alert.html text="You should have a working LaTeX setup" %}
<!-- **Pre-requisite:** You should have a working LaTeX setup.  -->

## Install Skim 

With [Homebrew](brew.sh)

```shell
brew install --cask skim
```
Or download the dmg file of the current version(as of writing latest version is v1.6.8)
from [Skim's website](https://skim-app.sourceforge.io).

## Install VimTeX

Using [vim-plug](https://github.com/junegunn/vim-plug) plugin manager we add the following
line to `.vimrc` or `init.vim` or `init.lua`

```shell
Plug 'lervag/vimtex'
```

## Pdf preview 
Conversion between TeX and PDF is one of the most common operations while writing a
scientific document. Though it is possible to open the PDF file in one of the commercially available
PDF readers, a seamless integration with neovim(in our case) is appreciated. This is where
Skim comes into the picture. By default, Skim allows native, seamless integration with the
LaTex editor of choice. In our case, we can make VimTex interact with Skim with just a few
lines of config.

## Configurations 

### Minimal setup and Forward Search

We require the following lines to make VimTeX talk to Skim within neovim. This direction of
communication, is known as *forward search*. 

```shell
let g:tex_flavor='latex' # Default tex file format
let g:vimtex_view_method = 'skim' # Choose which program to use to view PDF file 
let g:vimtex_view_skim_sync = 1 # Value 1 allows forward search after every successful compilation
let g:vimtex_view_skim_activate = 1 # Value 1 allows change focus to skim after command `:VimtexView` is given

```
The forward search allows any change made in the TeX file automatically refreshes Skim to
reflect those changes in PDF. One of the other common uses is cursor sync between the TeX file and PDF. 
Setting `let g:vimtex_view_skim_sync` allows placing the cursor in some position in the Tex file sync with the same
position in the PDF after every successful compilation(`:VimtexCompile`). Setting `let g:vimtex_view_skim_activate`
allows to shift focus of control from neovim to Skim and bring it to foreground. 

### Inverse or Backward Search

So far there was only one channel of communication between neovim(editor) and Skim.
A backward communication is possible but it took quite bit of hacking to get it
to work. More on that read [this jdhao's post](https://jdhao.github.io/2021/02/20/inverse_search_setup_neovim_vimtex/). 
However, with the release of [VimTex v2.8](https://github.com/lervag/vimtex/releases/tag/v2.8),
it has become simple to setup. 

Consider a scenario where we are going through a paper and find an error, instead of going
back to source TeX file and finding the error location can be cumbersome. Using *backward
search*, we can go to the error location from PDF to TeX. For Skim, to activate *backward
search* press `shift` and `command` together and `click` the position in PDF using the
mouse. That location gets reflected in the editor in the background. For more information,
see `:h :VimtexInverseSearch`

Natively, every instance of neovim starts a server [^1]. With Skim
as client and nvim as server, we can interact in that direction. 

In order to do so, in the preferances pane of Skim, navigate to `Sync` tab. There, in the
`PDF-TeX Sync support`, make `preset` as `custom`, `command` as `nvim`(use `vim` if you
use vim editor), and set `arguments` as `--headless -c "VimtexInverseSearch %line '%file'"`.
![]({{ site.baseurl }}/images/skim_setting.png "Skim preferances window")

**Important:** Skim must be started by VimTeX (either through compiler callback or explicitly via <leader>lv) 
for backward sync to work! (This is how Skim “knows” which neovim instance – terminal or GUI – to sync to.)

## Conclusion
With just four lines of settings in the `init.vim` file and a line in Skim preferances, we
can activate both forward and backward search features with VimTeX. 

## Footnotes
[^1]: In the nvim command line, run `:echo v:servername` to know the name of the server
