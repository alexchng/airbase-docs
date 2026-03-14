# Custom

You may use any base image that you wish on Airbase. However, to maximize compatibility during deployment, you should ensure that the image has the following:

1. The running user of the container shall not be `root`, and should have a UID of `999`.
2. Application should listen on port `$PORT`, which would be passed to the container at runtime.

[Managed Images](managed.md) [Help](../getting-help/)
