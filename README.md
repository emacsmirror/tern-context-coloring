# tern-context-coloring [![Build Status](https://travis-ci.org/jacksonrayhamilton/tern-context-coloring.svg?branch=master)](https://travis-ci.org/jacksonrayhamilton/tern-context-coloring)

Use [Tern][] as a back-end for context coloring.

Determines the lexical scope of functions and variables, and returns that
information to a Tern editor plugin which can translate it into code coloring.

[Tern]: http://ternjs.net/

## Emacs

This repository provides an Emacs plugin which collects scope data from Tern.
It uses the Emacs plugin [context-coloring][] to color buffers, which see.

[context-coloring]: https://github.com/jacksonrayhamilton/context-coloring

## Installation

Install the npm package globally:

`npm i -g tern-context-coloring`

Enable the plugin in `~/.tern-config` (recommended) or in `.tern-project`:

```json
{
  "plugins": {
    "context-coloring": {
      "charOffset": 1
    }
  }
}
```

(`"charOffset": 1` is for Emacs compatibility.  Consult the documentation of
your editor's context coloring plugin for the proper value of this option.)

[Configure Melpa][] in Emacs and install the plugin: `M-x package-install
tern-context-coloring`.  Enable the Tern context coloring Emacs plugin:

```lisp
(eval-after-load 'context-coloring
  '(tern-context-coloring-setup))
```

Also enable `tern-mode` and then `context-coloring-mode` when opening JavaScript
files:

```lisp
(add-hook 'js-mode-hook (lambda ()
                          (unless (eq major-mode 'json-mode)
                            (tern-mode)
                            (context-coloring-mode))))
```

[Configure Melpa]: http://melpa.org/#/getting-started

## Configuring

By default, only function scopes are recognized in the scope hierarchy.  Use
`"blockScope": true` to also recognize blocks (i.e. `let` and `const` scopes).

Tern usually treats all files as scripts, and thus top-level variables and
global variables are colored the same (as they share the same scope).  If the
Tern `modules` plugin is enabled (e.g. via the `es_modules` or `node` plugin),
then top-level variables will be colored indicating they are local to the
module.
