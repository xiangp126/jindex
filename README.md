### Illustrate
![](./gif/guide.gif)

#### clone `latch`
```git
git clone https://github.com/xiangp126/Latch
```

#### set up source code

put your source code into `OPENGROK_SRC_ROOT`, **per code per directory**

#### help message
```bash
sh oneKey.sh

[NAME]
    sh oneKey.sh -- setup OpenGrok through one key press

[SYNOPSIS]
    sh oneKey.sh [install | wrapper | summary | help] [PORT]

[EXAMPLE]
    sh oneKey.sh install
    sh oneKey.sh install 8081
    sh oneKey.sh wrapper
    sh oneKey.sh wrapper 8081
    sh oneKey.sh [help]
    sh oneKey.sh summary

[DESCRIPTION]
    install -> instlal OpenGrok using manual deploy method
    wrapper -> install OpenGrok using python wrapper
    summary -> print tomcat/OpenGrok guide and installation info
    help -> print help page
--
    'wrapper' and 'install' mode only differs in OpenGrok deploy method

[TIPS]
    run script needs root privilege but no sudo prefix
    default listen-port is 8080 if [PORT] was omitted

  ___  _ __   ___ _ __   __ _ _ __ ___ | | __
 / _ \| '_ \ / _ \ '_ \ / _` | '__/ _ \| |/ /
| (_) | |_) |  __/ | | | (_| | | | (_) |   <
 \___/| .__/ \___|_| |_|\__, |_|  \___/|_|\_\
      |_|               |___/
```

#### install routine
install `OpenGrok` using `manual` method

```
sh oneKey.sh install
```

or using python `wrapper`

```ruby
sh oneKey.sh wrapper
```

#### enjoy the site
> `indexer` was called in `oneKey`, you can also run `callIndexer` manually<br>
> take you server address as `127.0.0.1` for example<br>

then browser your `http://127.0.0.1:8080/source`

### Handle Web Service
#### on mac
```bash
catalina stop
catalina start
```

#### on linux
```bash
# repo main directory
sudo ./daemon stop
sudo ./daemon start
```

### Create Index Manually
#### new method
```bash
# repo main directory
./callIndexer
```

#### lagacy method (deprecated now)
```bash
# make index of source (multiple index)
./OpenGrok index [/opt/o-source]
                  /opt/source   -- proj1
                                -- proj2
                                -- proj3
```

### Introduction to Handy tools
#### auto pull
only support `Git` repository, auto re-indexing

```bash
# Go into your OPENGROK_SRC_ROOT
pwd
/opt/o-source

ls
coreutils-8.21      dpdk-stable-17.11.2 glibc-2.7           libconhash
dpdk-stable-17.05.2 dpvs                keepalived          nginx
```

add or remove item in `updateDir` of [autopull.sh](./autopull.sh)

```bash
updateDir=(
    "dpvs"
    "keepalived"
    "Add Repo Name according to upper dir name"
)
```

#### auto rsync
```bash
cat template/rsync.config
# config server info | rsync from
SSHPORT=
SSHUSER=
SERVER=
SRCDIR_ON_SERVER=
```

copy **template rsync.config** to current `rsync.config`

```bash
cp ./template/rsync.config .
vim rsync.config
# fix the information as instructed
```

#### add cron jobs
chage the time as you wish in [addcron.sh](./addcron.sh)

```bash
# Generate crontab file
cat << _EOF > $crontabFile
04 20 * * * $updateShellPath &> $logFile
_EOF
```

and change `updateShellPath` (the shell needs auto executed by cron) as you wish, default is `autopull.sh`

```bash
updateShellPath=$mainWd/autopull.sh
```

### Intelligence Window
#### how to launch?
hover over the item with mouse and press key `1` (numeric 1) to launch `Intelligence Window`

#### key shortcuts
- press key `b` to jump to previous appearance of item, and `n` to jump to the next one
- key `2` to **highlight** or unhighlight the item
- key `5` to **unhighlight all**
- `3` or `4` do the same function as key `2` but with different highlight color

### Problem Fix
#### `EZ-Zoom`
    If you use `EZ-Zoom` on `Chrome` with OpenGrok, make sure it's **100%** or OpenGrok will jump to the wrong line

#### dyld library issue - `mac`
```
dyld: Library not loaded: /usr/local/opt/gettext/lib/libintl.8.dylib
  Referenced from: /usr/local/bin/wget
  Reason: image not found
[1]    15507 abort      wget
```

brew install what was missing, here is `gettext`

```bash
brew install gettext
```

### License
The [MIT](./LICENSE.txt) License (MIT)