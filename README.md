# dotfiles

This repository contains my personal SketchyBar, Yabai and Skhd configurations.

![Screenshot2023-01-01](./README.assets/Screenshot2023-01-01.png)

## 0x01 Installation

```bash
# Installing yabai, skhd, jq and alacritty
brew tap homebrew/cask-fonts
brew install koekeishiya/formulae/yabai
brew install jq
brew install koekeishiya/formulae/skhd
brew install gh #(gh auth login for notifications)
brew install --cask sf-symbols
brew install --cask font-hack-nerd-font
brew install --cask alacritty

# setting tmux
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm


# Installing Fonts
git clone git@github.com:shaunsingh/SFMono-Nerd-Font-Ligaturized.git /tmp/SFMono_Nerd_Font
mv /tmp/SFMono_Nerd_Font/* $HOME/Library/Fonts
rm -rf /tmp/SFMono_Nerd_Font/
curl -L https://github.com/kvndrsslr/sketchybar-app-font/releases/download/v1.0.3/sketchybar-app-font.ttf -o $HOME/Library/Fonts/sketchybar-app-font.ttf

# macOS Settings
echo "Changing macOS defaults..."
defaults write com.apple.NetworkBrowser BrowseAllInterfaces 1
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
defaults write com.apple.spaces spans-displays -bool false
defaults write com.apple.dock autohide -bool true
defaults write com.apple.dock "mru-spaces" -bool "false"
defaults write NSGlobalDomain NSAutomaticWindowAnimationsEnabled -bool false
defaults write com.apple.LaunchServices LSQuarantine -bool false
defaults write NSGlobalDomain com.apple.swipescrolldirection -bool false
defaults write NSGlobalDomain KeyRepeat -int 1
defaults write NSGlobalDomain NSAutomaticSpellingCorrectionEnabled -bool false
defaults write NSGlobalDomain AppleShowAllExtensions -bool true
defaults write NSGlobalDomain _HIHideMenuBar -bool true
defaults write NSGlobalDomain AppleHighlightColor -string "0.65098 0.85490 0.58431"
defaults write NSGlobalDomain AppleAccentColor -int 1
defaults write com.apple.screencapture location -string "$HOME/Desktop"
defaults write com.apple.screencapture disable-shadow -bool true
defaults write com.apple.screencapture type -string "png"
defaults write com.apple.finder DisableAllAnimations -bool true
defaults write com.apple.finder ShowExternalHardDrivesOnDesktop -bool false
defaults write com.apple.finder ShowHardDrivesOnDesktop -bool false
defaults write com.apple.finder ShowMountedServersOnDesktop -bool false
defaults write com.apple.finder ShowRemovableMediaOnDesktop -bool false
defaults write com.apple.Finder AppleShowAllFiles -bool true
defaults write com.apple.finder FXDefaultSearchScope -string "SCcf"
defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false
defaults write com.apple.finder _FXShowPosixPathInTitle -bool true
defaults write com.apple.finder FXPreferredViewStyle -string "Nlsv"
defaults write com.apple.finder ShowStatusBar -bool false
defaults write com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool YES
defaults write com.apple.Safari AutoOpenSafeDownloads -bool false
defaults write com.apple.Safari IncludeDevelopMenu -bool true
defaults write com.apple.Safari WebKitDeveloperExtrasEnabledPreferenceKey -bool true
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2DeveloperExtrasEnabled -bool true
defaults write NSGlobalDomain WebKitDeveloperExtras -bool true
defaults write com.apple.mail AddressesIncludeNameOnPasteboard -bool false

# --- Remove previous links
rm -f "${HOME}"/.{yabai,skhd}rc

# --- Install configs
git clone https://github.com/zer0yu/dotfiles.git "${HOME}"/.config/dotfiles
ln -s "${HOME}/.config/dotfiles/yabai/yabairc" "${HOME}/.yabairc"
ln -s "${HOME}/.config/dotfiles/skhd/skhdrc" "${HOME}/.skhdrc"
ln -s "${HOME}/.config/dotfiles/sketchybar" "${HOME}/.config/sketchybar"
ln -s "${HOME}/.config/dotfiles/alacritty/alacritty.yml" "${HOME}/.config/alacritty/alacritty.yml"
ln -s "${HOME}/.config/dotfiles/alacritty/catppuccin/catppuccin-macchiato.yml" ${HOME}/.config/alacritty/catppuccin/catppuccin-macchiato.yml
ln -s "${HOME}/.config/dotfiles/tmux/.tmux.conf" "${HOME}/.tmux.conf"
ln -s "${HOME}/.config/dotfiles/tmux/sessions/" "${HOME}/.tmux/"

# 需要手动修改 .yabairc 和 .skhdrc 文件中的 .sh 脚本路径

# Start Services
brew services start skhd
brew services start fyabai
brew services start sketchybar
```



(optional) add to `.zshrc`

```shell
# Sketchybar interactivity overloads
function brew() {
  command brew "$@" 

  if [[ $* =~ "upgrade" ]] || [[ $* =~ "update" ]] || [[ $* =~ "outdated" ]]; then
    sketchybar --trigger brew_update
  fi
}

# Fancy sketchybar commands
function margin () {
  if [ $1 = "on" ]; then
    yabai -m config top_padding 20
    sketchybar --animate sin 30 --bar margin=10 y_offset=10 corner_radius=9
  else
    yabai -m config top_padding 10
    sketchybar --animate sin 30 --bar margin=0 y_offset=0 corner_radius=0
  fi
}

function zen () {
  ~/.config/sketchybar/plugins/zen.sh $1
}

function suyabai () {
  SHA256=$(shasum -a 256 /opt/homebrew/bin/yabai | awk "{print \$1;}")
  sudo sed -i '' -e 's/sha256:[[:alnum:]]*/sha256:'${SHA256}'/' /private/etc/sudoers.d/yabai
}
```





## 0x02 Keyboard Shortcuts



### 0x01 yabai(skhd)

### Reload / restart the yabai launch agent

<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>r</kbd>

### Show / hide items on desktop

<kbd>cmd</kbd> + <kbd>f3</kbd>

### Toggle full screen / up-full screen

<kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>space</kbd>

### Move Windows

| Action     | Key Combination                                              |
| ---------- | ------------------------------------------------------------ |
| Move left  | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>left</kbd> |
| Move right | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>right</kbd> |
| Move up    | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>up</kbd> |
| Move down  | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>down</kbd> |



### Focus Windows

| Action      | Key Combination                                    |
| ----------- | -------------------------------------------------- |
| Focus left  | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>left</kbd>  |
| Focus right | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>right</kbd> |
| Focus up    | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>up</kbd>    |
| Focus down  | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>down</kbd>  |



### Resize Windows

| Action       | Key Combination                                              |
| ------------ | ------------------------------------------------------------ |
| Resize left  | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>home</kbd> |
| Resize right | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>end</kbd> |
| Resize up    | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>pageUp</kbd> |
| Resize down  | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>pageDown</kbd> |



### Stack Windows

| Action      | Key Combination                                              |
| ----------- | ------------------------------------------------------------ |
| Stack left  | <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>left</kbd> |
| Stack right | <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>right</kbd> |



### Focus Windows (stack mode)

| Action     | Key Combination                                              |
| ---------- | ------------------------------------------------------------ |
| Focus up   | <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>up</kbd> |
| Focus down | <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>down</kbd> |



### Toggle float / Un-float mode window

<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>space</kbd>



### Move Windows (floating mode)

| Action                        | Key Combination                                       |
| ----------------------------- | ----------------------------------------------------- |
| Move left                     | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>left</kbd>     |
| Move right                    | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>right</kbd>    |
| full screen                   | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>up</kbd>       |
| place window to screen center | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>down</kbd>     |
| Move up left                  | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>home</kbd>     |
| Move down right               | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>end</kbd>      |
| Move up right                 | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>pageUp</kbd>   |
| Move down left                | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>pageDown</kbd> |



### Send Window to Spaces

| Action              | Key Combination                                     |
| ------------------- | --------------------------------------------------- |
| Send to prev space  | <kbd>ctrl</kbd> + <kbd>cmd</kbd> + <kbd>left</kbd>  |
| Send to next space  | <kbd>ctrl</kbd> + <kbd>cmd</kbd> + <kbd>right</kbd> |
| Send to space (1-9) | <kbd>ctrl</kbd> + <kbd>cmd</kbd> + <kbd>1 - 9</kbd> |



### Send Window to Monitors (Displays)

| Action               | Key Combination                                     |
| -------------------- | --------------------------------------------------- |
| Send to prev monitor | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>left</kbd>  |
| Send to next monitor | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>right</kbd> |



### Focus Monitors (Displays)

| Action          | Key Combination                                 |
| --------------- | ----------------------------------------------- |
| Focus monitor 1 | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>1</kbd> |
| Focus monitor 2 | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>2</kbd> |
| Focus monitor 3 | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>3</kbd> |



### Misc

| Action                      | Key Combination                                              |
| --------------------------- | ------------------------------------------------------------ |
| Window rotate clockwise     | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>r</kbd>               |
| Window rotate anticlockwise | <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>r</kbd> |
| Window Equalize size        | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>\|</kbd>              |
| Enable / Disable gaps       | <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>g</kbd>               |
| Float / Unfloat window      | <kbd>cmd</kbd> + <kbd>spcae</kbd>                            |



### Open APP

| Action         | Key Combination                                 |
| -------------- | ----------------------------------------------- |
| Open alacritty | <kbd>alt</kbd>+ <kbd>cmd</kbd> + <kbd>t</kbd>   |
| Open iTerm2    | <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>t</kbd> |
|                |                                                 |
|                |                                                 |



### Set the shortcut to open APP

1. open Automator(自动操作)

2. 选择 “选取文稿类型”,在创建界面选择”快速操作”

3. 创建“开启应用程序”

4. 设置快捷键

   “系统偏好设置” => "键盘" => "服务" => "通用" => 选中添加的程序， 设置快捷键。

Ref: https://blog.csdn.net/zhanglong_4444/article/details/105172109



或者使用 keyboard maestro 来自定义一些快捷操作



### 0x03  keyboard maestro

| Action       | Key Combination                                              |
| ------------ | ------------------------------------------------------------ |
| 清除格式粘贴 | <kbd>shift</kbd> + <kbd>alt</kbd> + <kbd>cmd</kbd> + <kbd>v</kbd> |



### 0x04 Tmux + Alacritty

| Action                                     | Key Combination                                              |
| ------------------------------------------ | ------------------------------------------------------------ |
| **基本操作**                               |                                                              |
| New Tab                                    | <kbd>cmd</kbd> + <kbd>t</kbd>                                |
| Close Tab                                  | <kbd>ctrl</kbd> + <kbd>d</kbd>                               |
| New vertical panel                         | <kbd>cmd</kbd> + <kbd>d</kbd>                                |
| Close vertical panel (关闭窗格)            | <kbd>cmd</kbd> + <kbd>w</kbd>                                |
| 水平分割窗格                               | <kbd>cmd</kbd> + <kbd>Return</kbd>                           |
| 访问左边的窗格                             | <kbd>cmd</kbd> + <kbd>h</kbd>                                |
| 访问左边的窗格                             | <kbd>cmd</kbd> + <kbd>Left</kbd>                             |
| 访问上面的窗格                             | <kbd>cmd</kbd> + <kbd>j</kbd>                                |
| 访问上面的窗格                             | <kbd>cmd</kbd> + <kbd>Up</kbd>                               |
| 问下面的窗格                               | <kbd>cmd</kbd> + <kbd>k</kbd>                                |
| 问下面的窗格                               | <kbd>cmd</kbd> + <kbd>Down</kbd>                             |
| 访问右边的窗格                             | <kbd>cmd</kbd> + <kbd>l</kbd>                                |
| 访问右边的窗格                             | <kbd>cmd</kbd> + <kbd>Right</kbd>                            |
| 按数字切换选项卡                           | <kbd>cmd</kbd> + <kbd>1</kbd> 到 <kbd>cmd</kbd> + <kbd>9</kbd> |
| 切换到最近一次访问的选项卡                 | <kbd>cmd</kbd> + <kbd>b</kbd>                                |
| 切换到上一个选项卡                         | <kbd>cmd</kbd> + <kbd>[</kbd>                                |
| 切换到下一个选项卡                         | <kbd>cmd</kbd> + <kbd>]</kbd>                                |
| 切换窗口                                   | <kbd>cmd</kbd> + <kbd>`</kbd>                                |
| **布局操作**                               |                                                              |
| 向左边的窗格推进                           | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>h</kbd>            |
| 向左边的窗格推进                           | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>Left</kbd>         |
| 向上面的窗格推进                           | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>j</kbd>            |
| 向上面的窗格推进                           | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>Up</kbd>           |
| 向下面的窗推进                             | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>k</kbd>            |
| 向下面的窗推进                             | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>Down</kbd>         |
| 向右边的窗格推进                           | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>l</kbd>            |
| 向右边的窗格推进                           | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>Right</kbd>        |
| 将窗格移动到指定的选项卡中                 | <kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>1</kbd> 到 <kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>9</kbd> |
| 缩放当前窗格                               | <kbd>cmd</kbd> + <kbd>z</kbd>                                |
| 使用预设置的五种布局重新调整窗格           | <kbd>cmd</kbd> + <kbd>shift</kbd> + <kbd>z</kbd>             |
| 修改选项卡名称                             | <kbd>cmd</kbd> + <kbd>,</kbd>                                |
| 输入 tmux 命令                             | <kbd>cmd</kbd> + <kbd>i</kbd>                                |
| 调整更大的字体                             | <kbd>cmd</kbd> + <kbd>+</kbd>                                |
| 调整更小的字体                             | <kbd>cmd</kbd> + <kbd>-</kbd>                                |
| 恢复默认大小的字体                         | <kbd>cmd</kbd> + <kbd>0</kbd>                                |
| **复制模式**                               |                                                              |
| 进入 Vi 复制模式，从上往下的方向查找关键词 | <kbd>cmd</kbd> + <kbd>f</kbd>                                |
| 进入 Vi 复制模式，从下往上的方向查找关键词 | <kbd>cmd</kbd> + <kbd>shift</kbd> +  <kbd>f</kbd>            |

补充：

v 按字选择

V 按行选择

Ctrl + v 按块状选择

Esc 取消选择

A 复制选择的文本追加到剪贴板，并退出 Vi 复制模式。

D 复制当前行到行尾的文本到剪贴板，并退出 Vi 复制模式。

y 复制选择的文本到剪贴板，并退出 Vi 复制模式。

Y 复制选择的文本到剪贴板，并保持 Vi 复制模式。

q 退出 Vi 复制模式

ref: https://juejin.cn/post/7152747783152697375



### 0x05 Arc Browser

| Action                               | Key Combination                                     |
| ------------------------------------ | --------------------------------------------------- |
| 打开URL或者搜索内容                  | <kbd>cmd</kbd> + <kbd>t</kbd>                       |
| 根据网页结构截图                     | <kbd>shift</kbd> + <kbd>2</kbd>                     |
| 同一页面右侧分割打开某个URL          | <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>=</kbd>   |
| 关闭分割页面                         | <kbd>ctrl</kbd> + <kbd>shift</kbd> + <kbd>-</kbd>   |
| 同一页面右侧分割打开点击的URL        | <kbd>alt</kbd> + 鼠标点击                           |
| 打开上次关闭的页面                   | <kbd>shift</kbd> + <kbd>cmd</kbd> + <kbd>t</kbd>    |
| 查看历史记录                         | <kbd>cmd</kbd> + <kbd>y</kbd>                       |
| 关闭所有窗口                         | <kbd>shift</kbd> + <kbd>cmd</kbd> + <kbd>w</kbd>    |
| 关闭当前页面                         | <kbd>cmd</kbd> + <kbd>w</kbd>                       |
| 切换 workspace                       | <kbd>ctrl</kbd> + <kbd>数字</kbd>                   |
| 打开 arc 设置（注意不是 chrome）     | <kbd>cmd</kbd> + <kbd>,</kbd>                       |
| 复制当前页面的 URL（仅URL）          | <kbd>shift</kbd> + <kbd>cmd</kbd> + <kbd>c</kbd>    |
| 复制当前页面的 URL（markdown 格式）  | <kbd>shift</kbd> + <kbd>option</kbd> + <kbd>c</kbd> |
| 把临时打开的小窗口加入到主浏览器里面 | <kbd>cmd</kbd> + <kbd>o</kbd>                       |
| Go back                              | <kbd>cmd</kbd> + <kbd>[</kbd>                       |
| Go forward                           | <kbd>cmd</kbd> + <kbd>]</kbd>                       |
| 显示or隐藏侧边栏                     | <kbd>cmd</kbd> + <kbd>s</kbd>                       |
| DeepL插件翻译                        | <kbd>alt</kbd> + <kbd>z</kbd>                       |
| Immersive Translate 插件全文翻译     | <kbd>alt</kbd> + <kbd>w</kbd>                       |



### 0x06 For Raycast

| Action                  | Key Combination               |
| ----------------------- | ----------------------------- |
| 打开Floating Notes      | <kbd>alt</kbd> + <kbd>.</kbd> |
| 显示/隐藏Floating Notes | <kbd>alt</kbd> + <kbd>,</kbd> |

 



## 0x07 Reference

https://github.com/FelixKratz/dotfiles

https://github.com/z20240/yabai

https://github.com/qingshan/dotfiles/blob/main/.tmux.conf

https://github.com/damianveltkamp/dotfiles/blob/master/tmux/.tmux.conf

