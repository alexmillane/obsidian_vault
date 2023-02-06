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
Moving workspaces with vim keys. Up and down can be changed in settings. Left and right with commands:
```bash
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-left "['<Primary><Alt>h']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-right "['<Primary><Alt>l']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-left "['<Primary><Shift><Alt>h']"
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-right "['<Primary><Shift><Alt>l']"
```


