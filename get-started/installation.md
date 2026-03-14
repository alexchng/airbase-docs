# Installation

## Prerequisite

As Airbase deploys containerized workloads, you’ll need to have Docker installed in your local machine.

See the following supported options for Docker installation:

* https://www.docker.com/products/docker-desktop/ (subscription required)
* https://github.com/abiosoft/colima
* https://rancherdesktop.io/

## Airbase CLI

Next, you’ll need the Airbase Command Line Interface (CLI).

NodeContainer

**Prerequisite**

Ensure that Node is installed in your system.

**Install**

```
sudo npm install --global @airbase/airbase-cli@containers
```

**Upgrading**

```
sudo npm remove --global @airbase/airbase-cli && sudo npm install --global @airbase/airbase-cli@containers
```

⚠️

Note that Container installation method is experimental!

**Install**

Copy the following shell script to `/usr/local/bin/airbase`:

```
#!/bin/bash

set -e

AIRBASE_SH_VERSION=0.1.0

dockerargs=()
[ -n "$AIRBASE_ENDPOINT" ] && dockerargs+=('-e' AIRBASE_ENDPOINT="$AIRBASE_ENDPOINT")
[ -n "$AIRBASE_ACCESS_KEY_ID" ] && dockerargs+=('-e' AIRBASE_ACCESS_KEY_ID="$AIRBASE_ACCESS_KEY_ID")
[ -n "$AIRBASE_SECRET_ACCESS_KEY" ] && dockerargs+=('-e' AIRBASE_SECRET_ACCESS_KEY="$AIRBASE_SECRET_ACCESS_KEY")
dockerargs+=('-e' AIRBASE_SH_VERSION="$AIRBASE_SH_VERSION")

set -u

airbaserc="$HOME/.airbaserc"
touch "$airbaserc"
pwd=$(pwd)

if [ $# -gt 0 ]; then
    case $1 in
        --upgrade)
            docker pull gdssingapore/airbase:cli
            exit 0
            ;;
    esac
fi

docker run \
    --rm \
    -it \
    -w "$pwd" \
    -v "$airbaserc":/root/.airbaserc \
    -v "$pwd":"$pwd" \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    ${dockerargs[@]+"${dockerargs[@]}"} \
    gdssingapore/airbase:cli \
    "$@"
```

Run the following to make the shell script executable

```
sudo chmod a+x /usr/local/bin/airbase
```

**Upgrading**

Run the following command:

```
airbase --upgrade
```

If it’s successfully installed, running `airbase --version` will print the current version of the CLI.

Optional: Checking the installed version

```
airbase --version
```

### Log in to the Airbase CLI

Run `airbase login` to connect your computer to the Airbase account.

```
airbase login
```

Visit the link with a web browser to complete the authentication process.

```
🛫 Log in to Airbase in your browser
  Visit: https://console.v2.airbase.sg/settings/credentials/cli?challenge=singleusetoken

Press Ctrl + C to cancel

======= after filling in the form =======

✅  Successfully logged in
```

> ⚠️ Note: If you’re not already logged in to your browser, you may be redirected to the homepage after logging in.
>
> Just click the link again and fill in the form as usual.
