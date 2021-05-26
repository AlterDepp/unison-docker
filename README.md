# unison-docker
Docker-compose environment for unison to solve the ocaml disaster

## Motivation
[Unison](http://www.cis.upenn.edu/~bcpierce/unison) is a great tool for synchronizing all your data. Unlike solutions like nextcloud, you always have full control of synchronized data. Unison performed great in the past, but in the near past, there is a problem with versions: the versions of unison and the used ocaml must match on client and server. Both! If you have the same operating system on both, you’re lucky. But even different versions of the same distribution do not work out of the box. You have to deal with flatpak/snap/ppa/aur/… . If you are using different distributions, you are lost. And unfortunately, if you want to build a matching unison yourself, you have to build ocaml, too.

## Solving the unison/ocaml disaster
So one solution is, to have a docker container for each operating system you need. It is quite easy. I started with an archlinux environment (on a ubuntu server). It is very lightweight, secure and easy to set up: 

1. Install docker-compose on your server.
2. Clone this repo or copy the files on your server.
3. You must set some environment variables. One way to do this is to create an *.env* file in the root directory (containing the *docker-compose.yaml* file:
```
USER=***            # user for syncing
GROUP=***           # main group name of the user
PASSWD=***          # password of the user
BASEDIR_HOST=***    # local data directory
BASEDIR_DOCKER=***  # data directory in the docker server
USER_UID=****       # UID of user
USER_GID=****       # GID of group
USERS_GID=****      # GID of group »users«
SUDO_GID=****       # GID of group »sudo«
HOSTNAME=****       # hostname of server
SSH_PORT=***        # local port mapped for ssh
```
Now run
```
docker-compose up -d
```
and that’s it. You should now be able to login with ```ssh -p ${SSH_PORT} ${USER}@${HOSTNAME}```.
On the host, from which you want to synchronize, you can additionally copy your ssh-key, so you do not have to enter your password anymore:
```
ssh-keygen
ssh-copy-id -p ${SSH_PORT} ${USER}@${HOSTNAME}
```
That’s it. Happy unisoning.

## Perspective
It should be easy to expand this for additional users or other operating systems.
Don’t forget to run ```docker-compose build``` after changing the *Dockerfile*.
