# ssh-private-key-buildpack

A Heroku buildpack for setting the SSH private key as part of the application
build. It's meant to be used as part of a setup using multiple buildpacks so
other buildpacks can authenticate with hosts using SSH keys, for instance to
install dependencies from private git repositories.

# Example usage

## Configure Multiple Buildpacks

The `ssh-private-key-buildpack` needs to run before any buildpack trying to get
SSH access:

    $ heroku buildpacks:add --index 1 https://github.com/Peque/ssh-private-key-buildpack.git

## Configure SSH Key

Set the private key environment variable `SSH_KEY` of your Heroku app (note
that the key needs to be base64 encoded).

    $ heroku config:set SSH_KEY="$(cat path/to/your/keys/id_rsa | base64)"

By default the buildback adds Github and Gitlab to `known_hosts`. However you
can configure your app to allow custom hosts, too. All that's needed is the set
`SSH_HOSTS` for you app to a comma-separated list of hosts:

    $ heroku config:set SSH_HOSTS="git@github.com,example.com"
