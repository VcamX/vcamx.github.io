---
layout: post
title: Something about syntax highlighting in Sublime Text 3
tags:
- English
- Syntax Highlighting
- Sublime Text
---

Sublime Text is definitely one of the most popular text editors in the world although some people complained about its less active development. Today I'm going to share something about its new Syntax Definition file format `.sublime-syntax` for syntax highlighting in Sublime Text 3.

**NOTE**: The minimum supported version is [Build 3103](https://www.sublimetext.com/blog/articles/sublime-text-3-build-3103) so unfortunately Sublime Text 2 doesn't have this powerful tool by now (maybe it'll support this soon?)

Recently I built [a tiny tool](https://github.com/VcamX/multiping) using [gRPC](http://www.grpc.io/) and [Protocol Buffers](https://developers.google.com/protocol-buffers/). But when I was writing `.proto` file I found the syntax highlighting powered by existing package from [Package Control](https://packagecontrol.io) in Sublime Text 3 is not so perfect.

![Broken syntax highlighting](https://cloud.githubusercontent.com/assets/2258983/13807059/2b43075e-eb9a-11e5-870c-907834cd3fb6.png)

The effect of syntax highlighting is not satisfying considering the syntax of Protocol Buffers is relatively simple. After some googling I decided to create a new one by myself.

This post won't repeat what has described in [the official documentation](https://www.sublimetext.com/docs/3/syntax.html) since it is clear and simple. We only focuses on key ideas and something need to know. So you may need to read the documentation first to get familiar with some basic concepts or terms.

<!--more-->

## General picture

If you have implemented parser or compiler before, writing syntax highlighting is very simple. The mechanism of syntax highlighting based on the same idea basically. And regexes plays an important role in it.

The `.sublime-syntax` file is natively [YAML](https://en.wikipedia.org/wiki/YAML) file and generally consists of three sections:

- **Header**: contains the basic information, like the name of language to syntax highlight and what files this `.sublime-syntax` file will applies to.
- **Contexts**: describes how to parse a file.
- **Variables** (optional): contains some reusable regexes within the `.sublime-syntax` file.

## Something need to note

The most important section in `.sublime-syntax` file is **contexts**, which decides how a file will be parsed. 

### 1. `main`

A `.sublime-syntax` file may contain many contexts but it must contain `main` context, which is the entry point of parsing.

### 2. `match`

Under each context, there may be one or more `match` patterns. Each `match` pattern is described by a regex. Something needs to note from the official documentation:

> If your regex includes the characters #, :, -, {, [ or > then you likely need to quote it. Regexes are only ever run against a single line of text at a time.
>
> When a context has multiple patterns, the leftmost one will be found. When multiple patterns match at the same position, the first defined pattern will be selected.

For example, if we have

```
context_1:
  - match: 'CCCC'     # Pattern 1
    ...
  - match: 'AAAA'     # Pattern 2
    ...
  - match: 'AAAABBBB'   # Pattern 3
    ...
```

Consider string `AAAABBBBCCCC`. Among three patterns, pattern 2 and pattern 3 have leftmost matched position and pattern 2 is defined before pattern 3, so pattern 2 will match first.

```
              pos    AAAABBBBCCCC
Pattern 1:     9             |--|
Pattern 2:     1     |--|
Pattern 3:     1     |------|
```

**NOTE**: Patterns within a context is in the same level, which means if a pattern is matched then the matching process of current context will continued until all text is parsed or certain matched pattern decides to `pop`.

Generally, the text will be fed to a tokenizer to tokenize first before parsing. Although we don't have tokenizer in Sublime Text, **regex** could give us almost the same (even more) power.

### 3. `push` and `pop`

`push` and `pop` allow us manipulate contexts in a stack. I think this is the most powerful feature, which reminds me of [recursive descent parser](https://en.wikipedia.org/wiki/Recursive_descent_parser).

### 4. `scope`

Sublime Text uses `.tmTheme` file to instruct how to colorize based on "annotations" given by `.sublime-syntax` file. And in `.sublime-syntax` file, "annotation" is defined by different types of scope:

- `meta_scope`
- `meta_content_scope`
- `scope`

Also see [*12.4 Naming Conventions*](https://manual.macromates.com/en/language_grammars) of TextMate documentation and [scopes for sublime text schemes](https://gist.github.com/alehandrof/5361546) summed up by [alehandrof](https://gist.github.com/alehandrof).

### 5. `include`

`include` make us easy to reuse contexts easily and make it more readable. Also note that `include` inserts patterns of the context into another.

For example, 

```
greet:
  - include: hi
  - match: 'hello'
    ...
hi:
  - match: 'hi'
  ...
```

is the same as

```
greet:
  - match: 'hi'
  ...
  - match: 'hello'
  ...
```

## Summary

Besides the documentation you can also check syntax definition files shipped with Sublime Text 3 and [my one](https://github.com/VcamX/protobuf-syntax-highlighting). If you wanna create your own syntax highlighting or migrate from the old syntax definition format to the one, I hope this post will be helpful to you. If there's any typo or error or anything else, you could leave your comments below :D

---

P.S. The documentation about language syntax of Protocol Buffers is a little vague so I have to read the source code of Protocol Buffers parser to find keywords or something else. Also, if you're writing Protocol Buffers using Sublime Text 3, try it out :P 

[**VcamX/protobuf-syntax-highlighting**](https://github.com/VcamX/protobuf-syntax-highlighting)

Let's see the result of my syntax highlighting:

![Nice syntax highlighting](https://cloud.githubusercontent.com/assets/2258983/13807043/22119e5c-eb9a-11e5-863f-dbbc22ba6182.png)
