# Anarcho-Tech NYC: Icecast [![Build Status](https://travis-ci.org/AnarchoTechNYC/ansible-role-icecast.svg?branch=master)](https://travis-ci.org/AnarchoTechNYC/ansible-role-icecast)

An [Ansible role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html) for installing an [Icecast](https://icecast.org/) server. Notably, this role has been tested with [Raspbian](https://www.raspbian.org/) on [Raspberry Pi](https://www.raspberrypi.org/) hardware. This role's purpose is to make it simple to install an Icecast (Internet radio) server in any of a number of flexible configurations, such as authenticated relays.

# Configuring Icecast

To configure your Icecast server instance, use any number of variables that map nearly one-to-one to the configuration directives described in [Icecast's Config File documentation page](https://icecast.org/docs/icecast-2.4.1/config-file.html). Configuration directive groups are their own dictionaries, and directives that can accept more than one value are specified as a list.

Some examples may prove helpful:

1. Simple Icecast server with default for all values except some user-supplied passwords and the server's public-facing hostname:
    ```yaml
    icecast_hostname: radio.example.com
    icecast_admin_password: ChangeMe
    icecast_source_password: ChangeMe
    ```
    This configuration will bring up your Icecast server on its default port `8000`. You can then broadcast through it by setting up a [source client](https://icecast.org/apps/) to send your stream to it using the password `ChangeMe` (which, obviously, you should change).
1. Simple Icecast server bound to the local host only and listening on the alternative HTTP port:
    ```yaml
    icecast_listen_sockets:
        - port: 8080
          bind_address: 127.0.0.1
    ```
    Note that `icecast_listen_sockets` is a list, meaning you can open multiple sockets. If a socket doesn't include the `bind_address` key, it will default to all interfaces (`0.0.0.0`).
1. Simple Icecast server with custom server metadata and a higher number of listeners (clients) and broadcasters (sources) allowed to connect:
    ```yaml
    icecast_location: Somewhere in Cyberspace
    icecast_admin_email: dj@radio.example.org
    icecast_default_stream: /the_alternative
    icecast_limits_clients: 9000
    icecast_limits_sources: 10
    ```

See the comments in the [`defaults/main.yaml`](defaults/main.yaml) file for additional details.
