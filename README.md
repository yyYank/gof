# gof

Go Fuzzy

![](http://i.imgur.com/TGZJyGV.gif)

[Open files in Vim directly (inside Vim terminal)](#vim-terminal-api)

![](https://i.imgur.com/pRhl9o3.gif)

## Installation

    $ go get github.com/mattn/gof

## Feature

* Faster and startup
* Working on windows

## Usage

* Glob files and edit the selected file with vim.

```sh
$ vim `gof`
```

* Run gof and type `CTRL-O`, then start to edit with editor.

```sh
$ gof
```

* Read from stdin

```sh
$ find /tmp | gof
```

## Key Assign

|Key                                               |Description                         |
|--------------------------------------------------|------------------------------------|
|<kbd>CTRL-K</kbd>,<kbd>ARROW-UP</kbd>             |Move-up line                        |
|<kbd>CTRL-J</kbd>,<kbd>ARROW-DOWN</kbd>           |Move-down line                      |
|<kbd>CTRL-A</kbd>,<kbd>HOME</kbd>                 |Go to head of prompt                |
|<kbd>CTRL-E</kbd>,<kbd>END</kbd>                  |Go to trail of prompt               |
|<kbd>ARROW-LEFT</kbd>                             |Move-left cursor                    |
|<kbd>ARROW-RIGHT</kbd>                            |Move-right cursor                   |
|<kbd>CTRL-O</kbd>                                 |Edit file selected                  |
|<kbd>CTRL-I</kbd>                                 |Toggle view header/trailing of lines|
|<kbd>CTRL-L</kbd>                                 |Redraw                              |
|<kbd>CTRL-U</kbd>                                 |Clear prompt                        |
|<kbd>CTRL-W</kbd>                                 |Remove backward word                |
|<kbd>BS</kbd>                                     |Remove backward character           |
|<kbd>DEL</kbd>                                    |Delete character on the cursor      |
|<kbd>CTRL-Z</kbd>                                 |Toggle selection                    |
|<kbd>Enter</kbd>                                  |Decide                              |
|<kbd>CTRL-D</kbd>,<kbd>CTRL-C</kbd>,<kbd>ESC</kbd>|Cancel                              |

## Options

|Option        |Description                      |
|--------------|---------------------------------|
|-c            |Cat the selected file            |
|-e            |Edit the selected file           |
|-             |Remove the selected file         |
|-l            |Launcher mode                    |
|-x            |Exit code for cancel (default: 1)|
|-d [path]     |Specify root directory           |
|-t            |Open via Vim's Terminal API      |
|-tf [funcname]|Terminal API's function name     |

## Launcher Mode

Put `~/.gof-launcher`

```
[name]	[command]
```

`name` and `command` should be separated by TAB. `gof -l` launch `command`s for selected `name`. Below is a my `.gof-launcher` file.

```
Vim	gvim
Emacs	emacs
GIMP	gimp
```

## Vim Terminal API

* `gof -t` or `gof -tf [prefix]` opens selected files in Vim using [Terminal
  API](https://vim-jp.org/vimdoc-en/terminal.html#terminal-api).  This option is
  ignored when `-l`, `-e`, `-c`, `-r`, or 1 or more non-option argument were
  supplied

* If you want to add `-t` option automatically whether you are inside Vim
  terminal or not, you can define alias like this

```sh
gof() {
  if [ "$VIM_TERMINAL" ]; then
    command gof -t "$@"
  else
    command gof "$@"
  fi
}
```

* If you are familiar with Vim script, you may want to send `["call", "[funcname]", "[file information]"]` instead of `["drop", "[filename]"]`. You can use `gof -tf [funcname]` to send `call` command

```
[file information] = {
  "filename": [relative filename path (string)],
  "fullpath": [absolute filename path (string)],
  "root_dir": [root directory (string)]
}
```

* You can define utility Vim command `:Gof`. Quickly calls `gof -t` command and
  opens selected files in Vim buffer

```vim
if executable('gof')
  command! -nargs=* Gof term ++close gof -t
endif
```

![](https://i.imgur.com/jvfuOxh.gif)

* Please try [vargs](https://github.com/tyru/vargs) if you want to communicate easily with Vim terminal API from shell

## License

MIT

## Author

Yasuhiro Matsumoto (a.k.a mattn)
