# install the component first and overwrite!!!

---

### 1. Uninstall **fish**

The package name is usually just `fish`. Run:

```bash
sudo apt remove --purge fish
```

If you want to remove leftover config files too (they live in `~/.config/fish`):

```bash
rm -rf ~/.config/fish
```

---

### 2. Install **zsh**

On Ubuntu/Debian:

```bash
sudo apt update
sudo apt install zsh
```

Verify it’s installed:

```bash
zsh --version
```

---

### 3. Make zsh your default shell

Run:

```bash
chsh -s $(which zsh)
```

Then **log out and back in** (or restart terminal). Now zsh should greet you.

---

### 4. Install a plugin manager

You’ll want something to handle plugins. The most popular is **Oh My Zsh**:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

This will install zsh config at `~/.zshrc`.

---

### 5. Add useful plugins

Inside your `~/.zshrc`, there’s a line like:

```zsh
plugins=(git)
```

You can add more. Here are some highly useful ones:

* `z`: jump quickly to frequently used directories
* `sudo`: press `Esc` twice to prefix last command with sudo
* `fzf`: fuzzy finder for searching history/files
* `zsh-autosuggestions`: command suggestions as you type
* `zsh-syntax-highlighting`: highlight commands for clarity

For the last two (not included by default), install manually:

```bash
# Autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

# Syntax highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

# fzf
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
```

Then edit your `.zshrc`:

```zsh
plugins=(git z zsh-autosuggestions zsh-syntax-highlighting fzf sudo)
```

Reload config:

```bash
source ~/.zshrc
```

---

### 6. Optional: pick a theme

Oh My Zsh comes with themes. A famous one is **agnoster**, but many prefer **powerlevel10k**:

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Set in `~/.zshrc`:

```zsh
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Then reload and it’ll guide you through a setup wizard.

---

### Forward-thinking tip

Zsh is powerful, but don’t overload it with plugins—it slows startup. Keep 3–5 essentials and learn zsh’s built-in features (`**` for recursive globbing, `=command` to expand paths, history expansion with `!!` and `!$`). That mastery matters more than shiny extras.

---

Do you want me to prepare a **minimal, optimized zsh plugin setup** file for you (`.zshrc`) so you can just drop it in?

### Make vim as the default editor

You’re asking two things:

1. How to set **vim** as the default editor for your whole environment.
2. Why **nano** shows up when you edit a Git commit, even though you want vim.

---

### 1. Set vim as the default editor system-wide

There are a few layers of “who decides what editor to use.” You’ll want to cover them all:

* **General environment**: set the `EDITOR` and `VISUAL` environment variables in your shell config (`~/.bashrc`, `~/.zshrc`, or `~/.profile`).

  ```bash
  export EDITOR=vim
  export VISUAL=vim
  ```

  Reload with:

  ```bash
  source ~/.bashrc
  ```

* **System-wide (Debian/Ubuntu, including WSL)**:
  Debian systems use the `alternatives` mechanism. Run:

  ```bash
  sudo update-alternatives --config editor
  ```

  Pick `vim.basic` (or whichever vim you like). This ensures all programs that respect the system editor use vim.

---

### 2. Why Git uses nano

Git looks for an editor in this order:

1. `git config core.editor`
2. `$GIT_EDITOR` environment variable
3. `$VISUAL` environment variable
4. `$EDITOR` environment variable
5. Falls back to system default (`/etc/alternatives/editor`, often nano on Ubuntu/WSL).

That’s why you still get nano—it’s the fallback.

To fix it just for Git:

```bash
git config --global core.editor "vim"
```

---

### Summary

* Set env vars in shell → `EDITOR` and `VISUAL`
* Configure system editor → `update-alternatives`
* Configure Git editor → `git config --global core.editor "vim"`

That triple-shot guarantees you’ll never be surprised by nano again.

---

Forward-thinking tip: sometimes you’ll want a GUI editor for Git (like VS Code) but vim for everything else. In that case, you can set:

```bash
git config --global core.editor "code --wait"
```

so you get both worlds depending on context.

Do you want me to show you how to test which editor Git is actually picking up right now?



