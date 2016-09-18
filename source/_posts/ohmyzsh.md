---
title: OhMyZsh
layout: post
category: setup
date: 2016-04-12
tags: [ohmyzsh, zsh]
---

## 설치 

``` bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
vi ~/.zshrc
```

테마를 수정한다.
`ZSH_THEME="agnoster"` # (this is one of the fancy ones)

테마가 특수한 문자 코드를 사용하기 때문에 패치된 폰트가 필요하다. [VIM](posts/2016/nvim/) 설정에서도 추가적인 폰트를 필요하므로 그 쪽을 참고한다.

안이쁘면 코딩 능력 발현이 안되므로 iTerm2의 설정에 들어가서 색상팔레트를 설정한다. 필자는 `Tango Dark`로 설정한다.

refs:
<https://github.com/robbyrussell/oh-my-zsh>
