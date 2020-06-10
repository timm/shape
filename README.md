<a name=top>
<h1 align=center>
   <a href="https://github.com/timm/blob/master/shape/README.md#top">
     SHape = Simple HAck 4   Programs + documeEntation
   </a>
</h1>
<p align=center>
   <a href="https://github.com/timm/shape/blob/master/LICENSE">license</a>
   :: <a href="https://github.com/timm/shape/blob/master/INSTALL.md#top">install</a>
   :: <a href="https://github.com/timm/shape/blob/master/CODE_OF_CONDUCT.md#top">contribute</a>
   :: <a href="https://github.com/timm/shape/issues">issues</a>
   :: <a href="https://github.com/timm/shape/blob/master/CITATION.md#top">cite</a>
   :: <a href="https://github.com/timm/shape/blob/master/CONTACT.md#top">contact</a>
</p>
<p align=center>
   <img width=600 src="https://github.com/timm/misc/blob/master/odd/etc/img/solidgallery.gif">
</p>
<p align=center>
   <img src="https://img.shields.io/badge/language-lua-orange">
   <img src="https://img.shields.io/badge/purpose-ai,se-blueviolet">
   <img src="https://img.shields.io/badge/platform-mac,*nux-informational">
   <a href="https://travis-ci.org/github/sehero/lua"> <img src="https://travis-ci.org/timm/shape.svg?branch=master"></a>
   <a href="https://zenodo.org/badge/latestdoi/263210595"> <img src="https://zenodo.org/badge/263210595.svg" alt="DOI"></a>
   <a href='https://coveralls.io/github/aiez/lua?branch=master'><img src='https://coveralls.io/repos/github/timm/shape/badge.svg?branch=master' alt='Coverage Status' /></a>
</p>

# Simpler Sharing for Code + Documentation

After  coding for decades 
I find myself repeating a common set of practices whenever I write in
 Prolog, Python, Perl, LISP, Smalltalk, CoffeeScript, LUA, Gawk, etc etc.  _SHape_ bundles up all those practices in one simple and convenient package.

Use _SHape_ if you want:

- Fast install:
   - With _SHape_, code can be installed code without having to update config files all over your hard drive.
- Fast uninstall:
   - With _SHape_, code can be fully and easily unistalled, just by deleting the _SHapre_ directory
- Easy unit testing:
   - To be able to separate your code into many small files, each of which can be tested severely;
   - To write unit tests checked by Travis CI whenever your commit to the repo.
- Simple documentation
   - To write documentation, very quickly, using literature programming.

Literate programmers surround their code with test telling the _story_ of that code. The standard literate programming (LP) "3 file model" implements that
as two post-processors that convert:
(1) some LP file into
(2) a documentation files; and
(3) an excitable code file.
_SHape_ implement's a simpler model where developers:

1. _Write a doco file._ 
   - In our case, an .md files containing code blocks. 
   - This has the advantage that the code/documentation can be read directly without any post-processor.
2. _Build the code file_, as required, from the .md files. 
   - _SHape_ ccomments out anything that is not a code block, then writes the results to a `.var` directory.



