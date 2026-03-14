# Login



[Airbase CLI](./) Login

## Login

`airbase login` helps developers to authenticate easily with the Airbase service.

### Usage

```
airbase login
```

**Options**

* `--endpoint [endpoint]`: Specify the Airbase console endpoint to login

After running this command, users will be given a single-use link to register a new set of credentials for this device.

**Effect**

{% hint style="info" %}
💡

Credentials are saved in `~/.airbaserc`.

The Airbase CLI saves (and loads) credentials from a file in the user’s home directory, if environment variables are absent or unset.
{% endhint %}

**Sample Output**

```
🛫 Log in to Airbase in your browser
  Visit: https://console.v2.airbase.sg/settings/credentials/cli?challenge=singleusetoken

Press Ctrl + C to cancel

======= after filling in the form =======

✅  Successfully logged in
```

### Authenticating in CI/CD

{% hint style="danger" %}
⚠️

These variables are secret and should be treated as such. Never check them into version control.
{% endhint %}

In a CI/CD environment, it isn’t possible to authenticate and retrieve credentials interactively.

Instead, set these two environment variables using an appropriate secrets mechanism:

* `AIRBASE_ACCESS_KEY_ID` - the access key identifier from the Airbase Console
* `AIRBASE_SECRET_ACCESS_KEY` - the secret shown once during credential creation

[Airbase CLI](./) [Container](container.md)
