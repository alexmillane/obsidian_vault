#dotfiles #linux
# dotfiles repo
I followed the instructions [here](https://www.ackama.com/what-we-think/the-best-way-to-store-your-dotfiles-a-bare-git-repository-explained/). 

I added 
```bash
alias config='/usr/bin/git --git-dir=$HOME/.cfg/.git/ --work-tree=$HOME'
```
to my `.bashrc` such that the command `config` replaces git commands when working with dotfiles. 

# tmux
I followed this youtube video [here](https://www.youtube.com/watch?v=Yl7NFenTgIo)

Installed tpm as described in the project github here [here](https://github.com/tmux-plugins/tpm)
```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

# fcf
TODO

# Ubuntu
## Workspaces
Moving workspaces with vim keys:
```bash
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-down "['<Primary><Alt>j']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-up "['<Primary><Alt>k']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-left "['<Primary><Alt>h']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-right "['<Primary><Alt>l']"

gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-up "['<Primary><Shift><Alt>k']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-down "['<Primary><Shift><Alt>j']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-left "['<Primary><Shift><Alt>h']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-right "['<Primary><Shift><Alt>l']"
```





# Obsidian
Currently I'm syncing my obsidian with git. 
### App Image
Obsidian is delivered as an app image which makes it annoying to launch. I use [AppImageLuancher](https://www.makeuseof.com/add-appimages-to-linux-system-menu/) to improve this experience.

## Obsidian git plugin (abandoned)
I abandoned it because it required creating a wallet to use the plugin. If you wanna pick it back up again. Use this link [here](https://publish.obsidian.md/git-doc/04+Authentication) and [here](https://docs.kde.org/trunk5/en/kwalletmanager/kwallet5/introduction.html).
