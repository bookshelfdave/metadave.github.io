+++
tags = ["kubernetes","bash"]
math = false
date = "2017-01-06T09:48:32-05:00"
title = "A Kubernetes-aware bash prompt"
authors = ["Dave Parfitt"]
summary = "A Kubernetes-aware bash prompt"
truncated = true
+++


If you manage multiple Kubernetes deployments, this little trick can show you the current Kubernetes context as part of your bash prompt.


Edit your `~/.bash_profile` or `~/.bashrc` with the following:

```
function kube_context() {
    # could have used $?, but it was easier with a string compare
    local ctx=$(kubectl config current-context 2>&1)
    if [ "${ctx}" != "error: current-context is not set" ]; then
        echo -n "<${ctx}> "
    fi
}

Blue='\e[0;34m'         # Blue
White='\e[0;37m'        # White
BRed='\e[1;31m'         # Red
IWhite='\e[0;97m'       # White
LightBlue='\e[1;34m'

PS1="\[$Blue\]\t\[$BWhite\]\[$LightBlue\] \$(kube_context)\[$BRed\]\[$BRed\]\w\[\033[m\]\[$IWhite\]\$(__git_ps1)\[$White\]\$ "
```

The snippet above yields the following prompt:

![alt](https://i.imgur.com/xk7a98Y.png)

The key part of the `PS1` value above is `\$(kube_context)`, which _must_ start with the `\` character, or it won't refresh after it's initial run.
