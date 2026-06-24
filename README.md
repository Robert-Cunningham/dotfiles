# dotfiles

Personal config snapshots. These are **plain copies**, not symlinked — pull
files down onto a machine as needed and drop them into place manually.

```
mac/                          # current macOS machine
  vscode/
    settings.json             -> ~/Library/Application Support/Code/User/settings.json
    keybindings.json          -> ~/Library/Application Support/Code/User/keybindings.json
    snippets/                 -> ~/Library/Application Support/Code/User/snippets/
    extensions.txt            # `code --list-extensions` snapshot
  karabiner/
    karabiner.json            -> ~/.config/karabiner/karabiner.json
  iterm2/
    com.googlecode.iterm2.plist  -> ~/Library/Preferences/com.googlecode.iterm2.plist
                                    (stored as readable XML; iTerm reads it fine)
  zsh/
    .zshrc                    -> ~/.zshrc

linux/                        # old Linux (i3/yadm) machine, kept for reference
```

## Restore on a new Mac

```sh
VS="$HOME/Library/Application Support/Code/User"
cp mac/vscode/settings.json    "$VS/settings.json"
cp mac/vscode/keybindings.json "$VS/keybindings.json"
mkdir -p "$VS/snippets" && cp mac/vscode/snippets/*.json "$VS/snippets/"
cat mac/vscode/extensions.txt | xargs -L1 code --install-extension

cp mac/karabiner/karabiner.json "$HOME/.config/karabiner/karabiner.json"
cp mac/zsh/.zshrc "$HOME/.zshrc"

# iTerm2: quit iTerm first, then:
cp mac/iterm2/com.googlecode.iterm2.plist "$HOME/Library/Preferences/com.googlecode.iterm2.plist"
defaults read com.googlecode.iterm2 >/dev/null   # reload prefs cache
```

## Re-snapshot current Mac settings

```sh
VS="$HOME/Library/Application Support/Code/User"
cp "$VS/settings.json" "$VS/keybindings.json" mac/vscode/
cp "$VS/snippets/"*.json mac/vscode/snippets/
code --list-extensions | sort > mac/vscode/extensions.txt
cp "$HOME/.config/karabiner/karabiner.json" mac/karabiner/
cp "$HOME/Library/Preferences/com.googlecode.iterm2.plist" mac/iterm2/
plutil -convert xml1 mac/iterm2/com.googlecode.iterm2.plist
cp "$HOME/.zshrc" mac/zsh/.zshrc
```
