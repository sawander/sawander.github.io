## ssh key

## Setup gpg-agent for ssh authentication

- enable gpg-agent
```
echo "use-agent" >> ~/.gnupg/gpg.conf
```

- enable ssh support
```
echo "enable-ssh-support" >> ~/.gnupg/gpg-agent.conf
```

- manual setting gpg-agent ssh support
```
echo "IdentityAgent /run/user/1000/gnupg/S.gpg-agent.ssh" >> ~/.ssh/config
```


## Configure ssh support for gpg-agent

- Create an subkey for ssh authentiction in your gpg key
```
gpg --expert --edit-key <KEY_ID>
```

- Specify ssh keys managed by gpg-agent
- Add the keys to '~/.gnupg/ssh-control'. No need to use ssh-add
- Entries ares keygrips referring your private/public gpg key
```
gpg -K --with-keygrip
echo D9463A85F093811B887DE4B03229F4BB09CC8A22 >> ~/.gnupg/ssh-control
```


- Init gpg-agent
```
export GPG_TTY="$(tty)"
export SSH_AUTH_SOCK="$(gpgconf --list-dirs agent-ssh-socket)"
gpgconf --launch gpg-agent
```

## Setup ssh forwarding

- need an extra ssh socket on the remote machine
```
gpgconf --list-dir agent-ssh-socket
agent-ssh-socket:/run/user/1000/gnupg/S.gpg-agent.extra

gpgconf --list-dirs agent-extra-socket
```

- allow agent forwarding in '~/.ssh/config'
```
 Host <remote>
   ForwardAgent yes
```

- Add extra socket as remote server to '~/.ssh/config'
```
Host <remote>
  HostName server.domain 
  RemoteForward <socket_on_remote_box> <extra_socket_on_local_box>
```
