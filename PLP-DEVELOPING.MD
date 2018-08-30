# Development Notes for the Pi Lambda Phi fork
Things I've figured out along the way.
## Setup
Checkout into ~/go/src/github.com/mattermost/mattermost-{server, webapp, docker}.

Configure your git remotes like this:

* origin -> elementc/mattermost-{server, webapp, docker}
* upstream -> mattermost/mattermost-{server, webapp, docker}

You may need to add ~/go to a go path var.

In general, follow https://developers.mattermost.com/contribute/server/developer-setup/

## Updating the Server
1. cd mattermost-webapp && git fetch upstream && git rebase upstream/release-n.n
  * if rebase isnt good, you dont currently have any commits so just resetting master to point at the upstream head
1. cd mattermost-server && git fetc
h upstream && git rebase upstream/release-n.n
  * may need to rewrite patches during the rebase.
1. cd mattermost-server && sudo make nuke && make clean-docker && make && make run
  * do your dev testing
  * sudo make nuke is to cleanup config and old builds (see sudo make build-linux)
1. make stop
1. sudo make build-linux && make build-client && make package
1. cd ~/go/bin && file mattermost && ldd mattermost
  * make sure the linux executable is statically linked
1. cd ~/go/src/github.com/mattermost/mattermost-server/dist
1. scp mattermost-team-linux-amd64.tar.gz user@mattermost-host:~/mattermost-docker/app/
1. ssh to the mattermost host.
1. cd to the mattermost-docker Checkout
1. docker-compose build
1. docker-compose up -d
1. backup the .tar.gz file for future emergency rollbacks :)
1. backup the volumes/ dir to stow data from this service. Check the ssh user's command history.
