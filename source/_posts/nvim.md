---
title: Neovim
tags:
---

``` bash
brew install neovim/neovim/neovim
curl -fLo ~/.config/nvim/autoload/plug.vim --create-dirs \
		     https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
cd ~ && git clone https://github.com/deptno/.config.git
touch ~/.local.vim
```

만약 `.config` 디렉터리가 있다면 실패하게된다 이때는 레포에 직접 접속해서 `.config/nvim/init.vim`을 들고와서 넣어준다.

특정 폰트를 사용해야만 깨짐이 없으므로 패치된 폰트를 아래서 하나 받아 설치하고 iTerm2의 Non-ASCII Font를 설치한 폰트로 설정.
<https://github.com/ryanoasis/nerd-fonts> 너무 많아서 개인적으로는 그 중에 [들어본 폰트](https://github.com/ryanoasis/nerd-fonts/blob/master/patched-fonts/DroidSansMono/additional-variations/Droid%20Sans%20Mono%20for%20Powerline%20Nerd%20Font%20Plus%20Font%20Awesome%20Mono%20Windows%20Compatible.otf)를 설치했다.

``` zsh
brew unlink vim
cp -f /usr/local/bin/nvim /usr/local/bin/vi
cp -f /usr/local/bin/nvim /usr/local/bin/vim
```

path문제로 되지 않을 경우 `.zshrc`의 `PATH`를 수정하면된다

## trouble shooting

> E902: "eslint" is not an executable

js 파일 수정시 eslint 가 실행되야 하는데 없어서 생기는 문제다.

``` bash
npm -g install eslint
```

> requires Vim compiled with Python (2.6+ or 3.3+) support

YouCompleteMe와 같은 플러그인은 파이썬을 필요로한다.

```bash
brew install python3
pip3 install --upgrade neovim
```

[https://neovim.io/doc/user/provider.html]
