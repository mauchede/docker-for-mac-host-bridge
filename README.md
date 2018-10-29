# README

Emulate docker0 bridge for docker-for-mac

## Installation

This project works with [socat](https://linux.die.net/man/1/socat) and [sshuttle](sshuttle/sshuttle). These dependencies can be installed with [brew](https://brew.sh/):

```sh
brew install socat

brew install sshuttle
```

## Usage

```sh
# Start ssh server

docker run --name host-bridge --net host --read-only --rm --tmpfs /run:exec --volume ${HOME}/.ssh/id_rsa.pub:/root/.ssh/authorized_keys:ro mauchede/docker-for-mac-host-bridge:latest

# Start socat forwarding

socat TCP-LISTEN:22,reuseaddr,fork "EXEC:docker exec -i host-bridge 'socat STDIO TCP-CONNECT:127.0.0.1:22'"

# Start sshuttle

sshuttle --remote root@127.0.0.1 --ssh-cmd 'ssh -i ${HOME}/.ssh/id_rsa -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null' 172.17.0.0/8 192.168.65.0/24
```

__Note__: `socat` is mandatory because port forwarding is not allowed when a container uses `--net host`.

## Contributing

1. Fork it.
2. Create your branch: `git checkout -b my-new-feature`.
3. Commit your changes: `git commit -am 'Add some feature'`.
4. Push to the branch: `git push origin my-new-feature`.
5. Submit a pull request.

If you like / use this project, please let me known by adding a [★](https://help.github.com/articles/about-stars/) on the [GitHub repository](https://github.com/mauchede/dotfiles).

## Links

* [brew](https://brew.sh/)
* [docker-for-mac networking](https://docs.docker.com/docker-for-mac/networking/)
* [image "mauchede/docker-for-mac-host-bridge"](https://hub.docker.com/r/mauchede/docker-for-mac-host-bridge/)
* [socat](https://linux.die.net/man/1/socat)
* [sshuttle](sshuttle/sshuttle)
* [using socat to forward new ports via stdio in a running docker container](https://gist.github.com/RickyCook/663b9f4ae53d454f73bf)
