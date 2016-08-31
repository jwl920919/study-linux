# Ubuntu Setup (v14)

### 1. Default Setting

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vim git wget curl ssh
```
```
# register ssh key on Github
# install chrome dropbox
```

### 2. Apperarance Seiing

 - Font
 ```
 sudo apt-get install unity-tweek-tool
 ```
 
 - Appearance Setting
 ```
enable workspace
autoi hide launcher on
launcher icon size : 40
 ```

- Brighteness & Lock
```
Turn screen off when inactive for : 10 minutes
```

 - Shell : ZSH
```
sudo apt-get install zsh
curl -L http://install.ohmyz.sh | sh
```

 - Solazied Terminal
```
wget --no-check-certificate https://raw.github.com/seebi/dircolors-solarized/master/dircolors.ansi-dark
mv dircolors.ansi-dark .dircolors

git clone https://github.com/sigurdga/gnome-terminal-colors-solarized.git
cd gnome-terminal-colors-solarized
./set_dark.sh

# add this line into your .zshrc
# eval `dircolors ~/.dircolors`
```

### 3. Key

 - Default Key Configuration
```
super + del -> gnome-system-monitor (Custom)
super + e -> Home Folder (in Launchers)
shift + alt + d -> hud (in Launchers)
right alt -> compose key (in Typing)
Use compose key as input source switcher (in Typing)
```

 - Capslock as ESC
```
$ sudo apt-get install gnome-tweak-tool
gnome-tweak-tool > Typing > Make Caps Lock an additional ESC
```

#### 4. Korean

```
Language Supoort -> install Korean
Text Entry -> add Korean(Hangul) in Input source 
```

 - 한글 짤림 문제 (Optional)
```
sudo add-apt-repository -y ppa:jincreator/freetype && sudo apt update && sudo apt install -y libfreetype6
```

#### 5. Development Environment

 - Node.js
```
$ sudo add-apt-repository ppa:chris-lea/node.js
$ sudo apt-get update
$ sudo apt-get install python-software-properties python g++ make nodejs
$ sudo npm install -g bower supervisor grunt-cli karma karma-cli
```

 - MongoDB
```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
$ echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
$ sudo apt-get update
$ sudo apt-get install mongodb-org
$ sudo service mongod start

$ # add admin user 
$ sudo vi /etc/mongodb.conf auth config
$ sudo service mongod restart
```

 - C++
```
$ sudo apt-get install gcc g++ make cmake autoconf libtool
```

 - Java8
```
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
$ sudo apt-get install oracle-java8-set-default
```

 - MySQL 5.5
```
$ sudo apt-get install mysql
$ # install mysql workbench
```

 - Spring
```
$ # get STS 
$ tar -zxvf sts*
$ sudo mv sts-bundle /opt
$ cd /opt/sts-bundle/sts-3.5.1.RELEASE
$ sudo ln -s /opt/sts-bundle/sts-3.5.1.RELEASE/STS /usr/local/bin/sts

$ sudo vi /usr/share/applications/sts.desktop

[Desktop Entry]
Name=sts
Exec=/usr/local/bin/sts
Terminal=false
StartupNotify=true
Icon=/opt/eclipse/icon.xpm
Type=Application

$ # add extentions vrapper, color-theme, gradle support
$ # set emacs key binding as default key
```

 - Git
```
[alias]
        ci = commit -v
        ca = commit --amend
        cm = commit -m

        co = checkout
        com = checkout master

        ph = push
        pl = pull

        a = !git add . && git status
        aa = !git add --all . && git status

        s = status --short --branch
        st = status

        df = diff

        ra = remote add
        rm = remote remove
        ro = remote show origin -n

        rs = reset --hard
        rh = reset

        br = branch
        bv = branch -v
        bd = branch -d

        ls = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate
        lg = "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(blue)[%an]%Creset' --abbrev-commit"

        last = log -1 HEAD
        type = cat-file -t
        dump = cat-file -p

        me = config user.name

        today = "log --since=midnight --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(blue)[%an]%Creset' --abbrev-commit"
        yesterday = "log --since yesterday --until=midnight --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(blue)[%an]%Creset' --abbrev-commit"

        outgoing = "log --pretty=oneline --abbrev-commit --graph @{u}.."
        incoming = "!git fetch && git log --pretty=oneline --abbrev-commit --graph ..@{u}"
```
