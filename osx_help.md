## create new file

> touch xxx

## install homebrew

- sudo mkdir /usr/local/Homebrew

- sudo git clone https://mirrors.ustc.edu.cn/brew.git /usr/local/Homebrew

- sudo ln -s /usr/local/Homebrew/bin/brew /usr/local/bin/brew # if term output 'File exists' means/usr/local/bin already have brew, then delete file brew (sudo rm /usr/local/bin/brew)

- sudo mkdir -p /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core

- sudo git clone https://mirrors.ustc.edu.cn/homebrew-core.git /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core

- sudo chown -R $(whoami) /usr/local/Homebrew

- HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles

- brew update
