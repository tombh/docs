# Supported tags and respective `Dockerfile` links

-	[`0.6.4`, `latest` (*Dockerfile*)](https://github.com/nats-io/nats-docker/blob/59e0d88a686f727b15dda47db5323c3722e6d85a/Dockerfile)

For more information about this image and its history, please see the [relevant manifest file (`library/nats`)](https://github.com/docker-library/official-images/blob/master/library/nats) in the [`docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

# [NATS](https://nats.io): A high-performance cloud native messaging system.

![logo](https://raw.githubusercontent.com/docker-library/docs/master/nats/logo.png)

`nats` is a high performance server for the NATS Messaging System.

# Example usage

```bash
# Run a NATS server
# Each server exposes multiple ports
# 4222 is for clients.
# 8222 is an HTTP management port for information reporting.
# 6222 is a routing port for clustering.
# use -p or -P as needed.

$ docker run -d --name nats-main nats
[1] 2015/08/08 02:18:59.240582 [INF] Starting gnatsd version 0.6.4
[1] 2015/08/08 02:18:59.240694 [INF] Starting http monitor on port 8222
[1] 2015/08/08 02:18:59.240708 [INF] Listening for route connections on 0.0.0.0:6222
[1] 2015/08/08 02:18:59.240801 [INF] Listening for client connections on 0.0.0.0:4222
[1] 2015/08/08 02:18:59.240823 [INF] gnatsd is ready

...

# To run a second server and cluster them together..
$ docker run -d --name=nats-2 --link nats-main nats --routes=nats-route://ruser:T0pS3cr3t@nats-main:6222

# If you want to verify the routes are connected, try
$ docker run -d --name=nats-2 --link nats-main nats --routes=nats-route://ruser:T0pS3cr3t@nats-main:6222 -DV
[1] 2015/08/08 06:06:18.662453 [INF] Starting gnatsd version 0.6.4
[1] 2015/08/08 06:06:18.662524 [INF] Starting http monitor on port 8222
[1] 2015/08/08 06:06:18.662680 [INF] Listening for route connections on :6222
[1] 2015/08/08 06:06:18.662807 [INF] Listening for client connections on 0.0.0.0:4222
[1] 2015/08/08 06:06:18.662831 [INF] gnatsd is ready
[1] 2015/08/08 06:06:18.662862 [DBG] Trying to connect to route on nats-main:6222
[1] 2015/08/08 06:06:18.663579 [DBG] 172.17.0.52:6222 - rid:1 - Route connection created
[1] 2015/08/08 06:06:18.663647 [DBG] 172.17.0.52:6222 - rid:1 - Route connect msg sent
[1] 2015/08/08 06:06:18.664040 [DBG] 172.17.0.52:6222 - rid:1 - Registering remote route "ee35d227433a738c729f9422a59667bb"
[1] 2015/08/08 06:06:18.664133 [DBG] 172.17.0.52:6222 - rid:1 - Route sent local subscriptions
```

The server will load the configuration file below. Any command line flags can override these values.

## Default Configuration File

```conf
# Client port of 4222 on all interfaces
port: 4222

# HTTP monitoring port
monitor_port: 8222

# This is for clustering multiple servers together.
cluster {

  # Route connections to be received on any interface on port 6222
  port: 6222

  # Routes are protected, so need to use them with --routes flag
  # e.g. --routes=nats-route://ruser:T0pS3cr3t@otherdockerhost:6222
  authorization {
    user: ruser
    password: T0pS3cr3t
    timeout: 0.75
  }

  # Routes are actively solicited and connected to from this server.
  # This Docker image has none by default, but you can pass a
  # flag to the gnatsd docker image to create one to an existing server.
  routes = []
}
```

## Commandline Options

```bash
Server Options:
    -a, --addr HOST                  Bind to HOST address (default: 0.0.0.0)
    -p, --port PORT                  Use PORT for clients (default: 4222)
    -P, --pid FILE                   File to store PID
    -m, --http_port PORT             Use HTTP PORT for monitoring
    -c, --config FILE                Configuration File

Logging Options:
    -l, --log FILE                   File to redirect log output
    -T, --logtime                    Timestamp log entries (default: true)
    -s, --syslog                     Enable syslog as log method.
    -r, --remote_syslog              Syslog server addr (udp://localhost:514).
    -D, --debug                      Enable debugging output
    -V, --trace                      Trace the raw protocol
    -DV                              Debug and Trace

Authorization Options:
        --user user                  User required for connections
        --pass password              Password required for connections

Cluster Options:
        --routes [rurl-1, rurl-2]    Routes to solicit and connect

Common Options:
    -h, --help                       Show this message
    -v, --version                    Show version
```

# License

View [license information](https://github.com/nats-io/gnatsd/blob/master/LICENSE) for the software contained in this image.

# Supported Docker versions

This image is officially supported on Docker version 1.8.1.

Support for older versions (down to 1.0) is provided on a best-effort basis.

# User Feedback

## Documentation

Documentation for this image is stored in the [`nats/` directory](https://github.com/docker-library/docs/tree/master/nats) of the [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs). Be sure to familiarize yourself with the [repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md) before attempting a pull request.

## Issues

If you have any problems with or questions about this image, please contact us through a [GitHub issue](https://github.com/docker-library/nats/issues).

You can also reach many of the official image maintainers via the `#docker-library` IRC channel on [Freenode](https://freenode.net).

## Contributing

You are invited to contribute new features, fixes, or updates, large or small; we are always thrilled to receive pull requests, and do our best to process them as fast as we can.

Before you start to code, we recommend discussing your plans through a [GitHub issue](https://github.com/docker-library/nats/issues), especially for more ambitious contributions. This gives other contributors a chance to point you in the right direction, give you feedback on your design, and help you find out if someone else is working on the same thing.