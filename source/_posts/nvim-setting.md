---
title: nvim-setting
date: 2021-09-18 15:33:54
tags:
  - neovim
  - linux
  - editor
  - tip
---
# Reason
  note:version需在0.5以上,因为neovim从5.0版本开始支持了treesistter(增量语法)和lua

# Why use neovim

  - 我发现我总是在进行代码的来回跳转，我需要记住大致的行数，
  大致的位置，我不想把我不多的精力浪费在此,而vim擅长文件跳转
  - vim有很多的编辑技巧,可是仅仅在编辑文本方面,和IDE之间的功能相差还是很大
  - vim可以实现ide的所有功能，但需要耗费的时间，精力成本,并不能在较短的时间里得到回报
  - neovim是在解决此痛点的,在版本0.5之后，引入了treesistter,lua,使vim更容易扩展
  - 我相信less is more,一切只为更友好的开发环境
  
# 剪切板
  - 将剪切板使用系统剪切板,让文本操作不影响剪切板
  ```lua
  vim.opt.clipboard = {'unnamed', 'unnamedplus'}
  --friendly registers
  map('n','d','"dd')
  map('v','d','"dd')
  map('n','x','"xx')
  map('v','x','"xx')
  map('n','c','"cc')
  map('v','c','"cc')
  map('n','<BS>','"cciw')
  ```

# 文件查找
  使用telescope的find_files配置
  ```lua
  map('n', '<c-p>',      [[<cmd>lua require'j.plugins.telescope'.find_files()<cr>]])
  ```

# 语言服务lsp
  vue - volar
  ```lua
  require('lspconfig').vuels.setup{
  on_attach = function(client, bufnr)
    -- Disable the document formatting for vuels because we want to use efm 
    -- with ESLint
    client.resolved_capabilities.document_formatting = false

    require('j.plugins.lsp').on_attach(client, bufnr)
  end,
  capabilities = require('j.plugins.lsp').capabilities,
  settings = {
    vetur = {
      experimental = {
        templateInterpolationService = true,
      },
      validation = {
        templateProps = true,
      },
      completion = {
        tagCasing = 'initial',
        autoImport = true,
        useScaffoldSnippets = true,
      },
    },
  },
  flags = {
    debounce_text_changes = 150,
  },
}

  ```
  

# 一些tip
  { - 上一个段落
  } - 下一个段落
  gv 选中上次选中
  git 冲突查找
  ```lua
map('n', ']n', [[:call search('^\(@@ .* @@\|[<=>|]\{7}[<=>|]\@!\)', 
'W')<cr>]], {silent = true})
map('n', '[n', [[:call search('^\(@@ .* @@\|[<=>|]\{7}[<=>|]\@!\)', 'bW')<cr>]], {silent = true})
  ```
