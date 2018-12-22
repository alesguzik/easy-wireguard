# Easy WireGuard

This is a set of Bash scripts to easily manage a set of WireGuard servers and clients.

This configuration allows you to add a number of servers that are mesh connected.

This allows you to configure clients to access particular server.

## Installation

```bash
git clone https://github.com/ayufan/easy-wireguard
```

Or

```bash
git clone https://gitlab.com/ayufan/easy-wireguard
```

## Add server

```bash
./add-server scaleway scaleway.remote.hostname 192.168.60.1/24 192.168.60.2 192.168.60.127
```

The:

- `scaleway` is a name of server
- `scaleway.remote.hostname` is a remote endpoint
- `192.168.60.1/24` is an tunnel address of the server
- `192.168.60.2 192.168.60.127` the IP range from which the IPs are allocated to clients

## Add client

```bash
./add-client kamil-macbook
```

The:

- `kamil-macbook` the name of client

## Installing on Server

For each server follow the https://www.wireguard.com/install/:

```bash
$ sudo add-apt-repository ppa:wireguard/wireguard
$ sudo apt-get update
$ sudo apt-get install wireguard
```

## Configuring Server

```bash
./emit-server --shell scaleway
```

This will print a set of commands that enable VPN on start.
Just copy-paste it and voila, or:

```bash
./emit-server --shell scaleway | ssh root@scaleway.server
```

However, if you did install `easy-wireguard` on scaleway.server, you can also use:

```bash
./emit-server --up scaleway
./emit-server --down scaleway
```

## Configuring Client

There are number of ways to grab config

### 1. Grab config (and use it)

```bash
./emit-client scaleway kamil-macbook
```

This gets config for particular server.

### 2. Grab shell commands if your machine is client

```bash
./emit-client --shell scaleway kamil-macbook
```

### 3. Use QR-code

This is a new way to installing VPN config!

```bash
./emit-client --qr scaleway kamil-macbook
```

## Configure additional routes

It is possible to expose either on client, or on server additional routes.
Simply edit the `servers/server.conf` or `clients/client.conf` and modify `Routes=`:

```bash
Routes="192.168.0.0/24,192.168.20.0/24"
```

And re-install each client and server.

## Configure server to expose default gateway

Pass a default gateway interface via `DefaultGateway=` in `servers/server.conf`:

```bash
DefaultGateway=ens2
```

And re-install server.

## Use client config that routes all traffic via VPN simply add `--default`

```bash
./emit-client --default scaleway kamil-macbook
./emit-client --default --qr scaleway kamil-macbook
./emit-client --default --shell scaleway kamil-macbook
```

## Additional configurations

There's number of additional configurations that you might be interested in:

- `Routes=` additional routes
- `DNS=` configured DNS server used by clients
- `ListenPort=` listen address for servers (and optionally clients)
- `PersistentKeepalive=` keep connections alive
- ...

## Author

Kamil Trzciński, 2018

## License

MIT