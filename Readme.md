# [C](#)onqure [o](#)f  [C](#)ompletion

COC is a **fast**, **reliable** and **flexible** completion framework of
[neovim](https://github.com/neovim/neovim).

It also tries hard to have better support for web development and completion
specifications in [language server protocol](https://github.com/Microsoft/language-server-protocol)

W.I.P.🐒

## Features

* Async generate complete items in parallel.
* Smart case fuzzy match with score by default.
* Minimal configuration required to work.
* Always respect your `completeopt`.
* Support `text_edit` and `snippet` described in LSP 2.0.
* Custom sources using vim script.

## Install

Take [dein.vim](https://github.com/Shougo/dein.vim) as example:

``` vim
 call dein#add('neoclide/coc.nvim', {
    \ 'build': 'make'
    \})
```

[nodejs](http://nodejs.org/) version > 8.0 && neovim version > 0.3.0 is required.

See [trouble Shooting](#trouble-shooting) if you have runtime issue. 

### Set trigger for completion

**Tab is awesome**

``` vim
function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~ '\s'
endfunction

inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
```

**Use custom key**

``` vim
imap <c-space> <Plug>(coc_start)
```

## Sources

Native sources are implemented in javascript and are enabled by default.


Name         | Description                                             | Use cache   | Supported filetypes
------------ | -------------                                           | ------------|------------
`around`     | Words of current buffer.                                | ✗           | all
`buffer`     | Words of none current buffer.                           | ✓           | all
`dictionary` | Words from files of local `dictionary` option.          | ✓           | all
`file`       | Filename completion, auto detected.                     | ✗           | all
`omni`       | Invoke `omnifunc` of current buffer for complete items. | ✗           | User defined
`word`       | Words from google 10000 english repo.                   | ✓           | User defined
`emoji`      | Eomji characters.                                       | ✓           | User defined
`module`     | Words of module names.                                  | ✗           | [Limited](/src/source/module_resolve)
`include`    | Full path completion for include file paths.            | ✗           | [Limited](/src/source/include_resolve)

Note: `module` & `path` source only have support for quite limited filetypes, need help.

## Configuration

<details>
  <summary>Here're some common complete configuration of vim:</summary>

``` vim
" user <Tab> and <S-Tab> to iterate complete item
inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"
" use <enter> to finish complete
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<cr>"

" Auto close preview window when completion is done.
autocmd! CompleteDone * if pumvisible() == 0 | pclose | endif

" The completeopt coc works best with, see `:h completeopt`
set completeopt=menu,preview
```
</details>


### Global variables

Name                        | Description                                               | Default
------------                | -------------                                             | ------------
`g:coc_fuzzy_match`         | Use fuzzy match for words                                 | 1
`g:coc_timeout`             | Timeout in milisecond for completion                      | 300
`g:coc_ignore_git_ignore`   | Ignore collect words from buffers that are git ignored    | 0
`g:coc_source_config`       | Configuration for coc sources, see `:h coc_source_config` | []
`g:coc_increment_highlight` | Enable highlight for increment search characters          | false
`g:coc_chars_guifg`         | Foreground color of user input in increment search        | white
`g:coc_chars_guibg`         | Background color of user input in increment search        | magenta

### Commands

Commands are used change the service status on the fly.

Name                 | Description
------------         | -------------
`:CocRefresh [name]` | Refresh `name` source, or all sources without argument.
`:CocToggle name`    | Toggle `name` source state (enable/disable).
`:CocDisable`        | Disable coc.nvim
`:CocEnable`         | Enable coc.nvim
`:Denite coc`        | Open coc sources in [denite.nvim](https://github.com/Shougo/denite.nvim) buffer.

## Trouble shooting

When you find the plugin is not working as you would expected, run command
`:checkhealth` and make use that output from `coc.nvim` are `OK`.

To get the log file, run shell command:

    node -e 'console.log(path.join(os.tmpdir(), "coc-nvim.log"))'

You can also use environment variable to change logger behaviour:

* `$NVIM_COC_LOG_LEVEL` set to `debug` for debug messages.
* `$NVIM_COC_LOG_FILE` set the file path of log file.

Note: Coc would disable itself when there is vim error during autocmd.

## Similar projects

* [deoplete.nvim](https://github.com/Shougo/deoplete.nvim)
* [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim)
* [vim-mucomplete](https://github.com/lifepillar/vim-mucomplete/)
* [nvim-completion-manager](https://github.com/roxma/nvim-completion-manager)
* [completor.vim](https://github.com/maralla/completor.vim)

## LICENSE

MIT
