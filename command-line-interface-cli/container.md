# Container

The `container` subcommand helps developers manage, deploy, and destroy their containers on Airbase.

### 🛠️ Prerequisites

Before using the `airbase container build` command, ensure you have the Docker Buildx plugin installed.

This is **not installed by default** after the initial SEED setup.

#### Install Docker Buildx

{% stepper %}
{% step %}
### Check if it’s already available

```
docker buildx version
```
{% endstep %}

{% step %}
### Install Buildx if not present

```
mkdir -p ~/.docker/cli-plugins
curl -SL https://github.com/docker/buildx/releases/latest/download/docker-buildx-$(uname -s)-$(uname -m) -o ~/.docker/cli-plugins/docker-buildx
chmod +x ~/.docker/cli-plugins/docker-buildx
```
{% endstep %}
{% endstepper %}

### Build

Builds an image based on the Dockerfile present in the local directory.

**Usage**

```
airbase container build
```

**Options**

* `--quiet`: Silence Docker Daemon build logs

**Sample Output**

```
#0 building with "colima" instance using docker driver
...
#17 exporting to image
#17 exporting layers done
#17 writing image sha256:fe7b1664171e462ccf46256d63c7734854e81b20f0a0f31058eaff33f0216eef done
#17 naming to local.airbase.sg/days:79a10d0728091802a7769fdeb0418ba5 done
#17 DONE 0.0s
#
Successfully built image local.airbase.sg/days:42b313a629a41f8f6e25344b30b9f08a
Run airbase container deploy to launch your application! 🛫
```

### Deploy

Deploy application based on the image.

By default, deploys the image built from `airbase container build`

**Usage**

```
airbase container deploy [environment]
```

{% hint style="info" %}
Airbase only supports amd64 images. Take extra care to specify `linux/amd64` when you are building images on an Apple Silicon device.
{% endhint %}

**Options**

* `--image [image]`: Specify the image to be deployed. Defaults to the local image built by `airbase container build`
* `--project <team/project>`: Specify the team and project handle to be deployed
* `--yes`: Skip confirmation prompt

#### Examples

**Deploying to a preview environment**

```
airbase container deploy feat-1234-preview-changes
```

**Deploying to a branch environment in GitLab CI/CD**

```
airbase container deploy --yes ${CI_COMMIT_REF_SLUG}
```

**Build and deploy custom x86 images to default (production) environment**

```
docker build --platform linux/amd64 --tag my-build .
airbase container deploy --image my-build
```

### Destroy

Destroys the selected environment of the current application.

**Usage**

```
airbase container destroy [environment]
```

**Arguments**

* `environment`: name of environment to deploy to (e.g. `default`)

**Options**

* `--yes`: skips confirmation prompt, useful in CI/CD environments
* `--project <team/project>`: Specify the team and project handle to be deployed

**Sample Output**

> An extra confirmation step will be required when destroying a production environment.

```
? Destroy NextJS Hello World Template: template-nextjs-helloworld - default (production) Yes
? template-nextjs-helloworld will be destroyed. This action is irreversible.
Type the project handle (template-nextjs-helloworld) to confirm: nope
Project name does not match
```

#### Examples

**Destroying a preview environment**

```
airbase container destroy feat-1234-preview-changes
```

**Destroying a branch environment in GitLab CI/CD**

```
airbase container destroy ${CI_COMMIT_REF_SLUG} --yes
```
