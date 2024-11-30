# Step 1: Install Neovim

    For Linux (Debian/Ubuntu):

sudo apt update
sudo apt install neovim

For macOS (using Homebrew):

    brew install neovim

    For Windows:
        Download the latest release from Neovim's GitHub page.
        Install the .msi file.

# Step 2: Install a Package Manager for Neovim

The most popular package manager for Neovim is lazy.nvim.

    Clone lazy.nvim:

git clone https://github.com/folke/lazy.nvim ~/.local/share/nvim/lazy/lazy.nvim

Add lazy.nvim to your Neovim configuration. Create a ~/.config/nvim/init.lua file and add the following:

    -- Lazy.nvim setup
    local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
    if not vim.loop.fs_stat(lazypath) then
        vim.fn.system({
            "git",
            "clone",
            "--filter=blob:none",
            "https://github.com/folke/lazy.nvim.git",
            lazypath,
        })
    end
    vim.opt.rtp:prepend(lazypath)

# Step 3: Install Python Support

    Ensure you have Python installed.
        Use python3 --version or python --version to check.
        If not installed, install it based on your OS:
            Ubuntu/Debian: sudo apt install python3
            macOS: brew install python
            Windows: Download from python.org.

    Install the Neovim Python client in your global Python environment:

    pip install pynvim

    If you use virtual environments (like venv), activate the environment before installing pynvim.

# Step 4: Configure Neovim for Python Development

Add the following to ~/.config/nvim/init.lua:

-- Lazy.nvim initialization
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
    vim.fn.system({
        "git",
        "clone",
        "--filter=blob:none",
        "https://github.com/folke/lazy.nvim.git",
        lazypath,
    })
end
vim.opt.rtp:prepend(lazypath)

require("lazy").setup({
    -- LSP Configuration
    {
        "neovim/nvim-lspconfig",
        config = function()
            local lspconfig = require("lspconfig")
            lspconfig.pyright.setup{}  -- Python LSP
        end,
    },
    -- Autocompletion
    {
        "hrsh7th/nvim-cmp",
        dependencies = {
            "hrsh7th/cmp-nvim-lsp",
            "hrsh7th/cmp-buffer",
            "hrsh7th/cmp-path",
            "hrsh7th/cmp-cmdline",
        },
        config = function()
            local cmp = require("cmp")
            cmp.setup({
                snippet = {
                    expand = function(args)
                        vim.fn["vsnip#anonymous"](args.body)
                    end,
                },
                mapping = {
                    ['<C-n>'] = cmp.mapping.select_next_item(),
                    ['<C-p>'] = cmp.mapping.select_prev_item(),
                    ['<CR>'] = cmp.mapping.confirm({ select = true }),
                },
                sources = {
                    { name = "nvim_lsp" },
                    { name = "buffer" },
                },
            })
        end,
    },
    -- Snippets (optional)
    {
        "hrsh7th/vim-vsnip",
    },
})

# Step 5: Install LSP and Tools

    Install pyright for Python language support:

sudo npm install -g pyright

If you don't have Node.js, install it via:

sudo apt install nodejs npm  # For Linux
brew install node            # For macOS

Open Neovim and run:

    :Lazy

    This will install all the plugins defined in your init.lua.

# Step 6: Verify Configuration

    Open a Python file in Neovim:

    nvim example.py

    Check if LSP is working:
        Hover over a function: K
        Go to definition: gd
        See diagnostics: :LspDiagnostics

    Check if autocomplete works by typing in a Python function.

