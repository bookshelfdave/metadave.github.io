+++
tags = ["git","bash"]
math = false
date = "2017-03-08T09:00:00-00:00"
title = "Automatically locking a git crypt repo"
authors = ["Dave Parfitt"]
summary = "Automatically locking a git crypt repo"
truncated = true
+++


I've been using [Git Crypt](https://www.agwa.name/projects/git-crypt/) to work with a few secure git repos [at work](https://mozilla.github.io/meao/). It works well and is easy to use. 

The one thing that occasionally wakes me up in the middle of the night is wondering if I've locked a repo when I'm finished with it. To help with this issue, I threw together a quick `bash` function using `at` and `wall`. I use this on both Debian Linux and OSX.

```
git_crypt_autolock() {
    PROJECT_ROOT=$(git rev-parse --show-toplevel)
    SLEEP_TIME="2 hours"
    echo "Automatically locking ${PROJECT_ROOT} in ${SLEEP_TIME}"
    at now + "${SLEEP_TIME}" <<EOF
cd ${PROJECT_ROOT} && git crypt lock
wall <<< "git crypt automatically locked in ${PROJECT_ROOT}"
EOF
}
```

Change the sleep time to whatever you need, it follows the `at` time specification. I'm sure this could be called via alias/git alias, but it works as is for me and keeps me sleeping at night.


#### A note on enabling the `atrun` daemon in MacOS:

 If you don't recieve notifications in OSX, make sure the `atrun` daemon is enabled.

See also: `man atrun`

On MacOS Sierra, this command enabled the atrun daemon for me:

```
sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.atrun.plist
```

> It's [non-trivial](https://github.com/julienXX/terminal-notifier/issues/200) to send a proper notification in OSX from an `at` script. Let me know if you get it working ;-)

