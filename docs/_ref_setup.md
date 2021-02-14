
Setting up a new workstation is easy. The only software required (docker, curl, git) is part of 
most linux distributions.


## Setup your workstation <small>on Debain/Ubuntu, [klick here for Windows / MacOS](setup/)</small>

Logged in with your regular user account (not root), open a terminal and execute:

<pre>
<code data-toggle="tooltip" data-placement="right" 
title="Install required packages"
>sudo apt-get update && sudo apt-get install -y docker.io curl git ssh</code>
<code data-toggle="tooltip" data-placement="left" 
title="Add your normal user to docker group so you can execute docker without root priveleges"
>sudo gpasswd -a $USER docker</code>
<code data-toggle="tooltip" data-placement="left" 
title="Set your user name for git commit messages"
>git config --global user.name "John Doe"</code>
<code data-toggle="tooltip" data-placement="left" 
title="Set your email address for git commit messages"
>git config --global user.email "johndoe@example.com"</code>
<code data-toggle="tooltip" data-placement="left" 
title="Store credentials to ~/.git-credentials"
>git config --global credential.helper store</code>
<code data-toggle="tooltip" data-placement="left" 
title="We suggest this flat directory as root for all of your projects"
>mkdir -p $HOME/Projects</code>
<code data-toggle="tooltip" data-placement="left" 
title="Create a ssh key to access git repositories. Select a good password. You'll have to type it whenever you push/pull."
>ssh-keygen -t ed25519 && cat $HOME/.ssh/id_ed25519.pub</code>
</pre>

The last command will print your *public ssh key*:

```
ssh-ed25519 AAA..............some..more..chars...........J3jAPTge jondoe@example
```

Copy-n-paste the whole line and add it to your favorite development platform:
- [github.com](https://github.com): <kbd>Personal settings</kbd> > <kbd>SSH and GPG Keys</kbd> > <kbd>New SSH key</kbd>
- [gitlab.com](https://gitlab.com): <kbd>User Settings</kbd> > <kbd>SSH Keys</kbd>

Finally install your favorite IDE. *Logout and login again* <small>(to have changed user permissions applied)</small>. That's it.

