# `leafnodes` Configuration Block

| Property | Description |
| :------  | :---- |
| `advertise` | Hostport `<host>:<port>` to advertise to other servers. |
| `authorization` | Authorization block. [**See Authorization Block section below**](#authorization-block). |
| `host` | Interface where the server will listen for incoming leafnode connections. |
| `listen` | Combines `host` and `port` as `<host>:<port>` |
| `no_advertise` | if `true` the leafnode shouldn't be advertised. |
| `port` | Port where the server will listen for incoming leafnode connections. |
| `remotes` | List of `remote` entries specifying servers where leafnode client connection can be made. |
| `tls` | TLS configuration block (same as other nats-server `tls` configuration). |

## Authorization Block

| Property | Description |
| :------  | :---- |
| `user` | Username for the Leafnode connection. |
| `password` | Password for the user entry. |
| `account` | Account this Leafnode connection should be bound to. |
| `timeout` | Maximum number of seconds to wait for Leafnode authentication. |
| `users` | List of credentials and account to bind to Leafnode connections. [**See User Block section below**](#users-block). |

### Users Block

| Property | Description |
| :------  | :---- |
| `user` | Username for the Leafnode connection. |
| `password` | Password for the user entry. |
| `account` | Account this Leafnode connection should be bound to. |

Here are some examples of using basic user/password authentication for Leafnodes:

Singleton mode:
```
leafnodes {
  port: ...
  authorization {
    user: leaf
    password: secret
    account: TheAccount
  }
}
```
With above configuration, if a soliciting server creates a Leafnode connection with url: `nats://leaf:secret@host:port`, then the accepting server will bind the leafnode connection to the account "TheAccount". This account need to exist otherwise the connection will be rejected.

Multi-users mode:
```
leafnodes {
  port: ...
  authorization {
    users = [
      {user: leaf1, password: secret, account: account1}
      {user: leaf2, password: secret, account: account2}
    ]
  }
}
```
With the above, if a server connects using `leaf1:secret@host:port`, then the accepting server will bind the connection to account `account1`.
If using `leaf2` user, then the accepting server will bind to connection to `account2`.

If username/password (either singleton or multi-users) is defined, then the connecting server MUST provide the proper credentials otherwise the connection will be rejected.

If no username/password is provided, it is still possible to provide the account the connection should be associated with:
```
leafnodes {
  port: ...
  authorization {
    account: TheAccount
  }
}
```
With the above, a connection without credentials will be bound to the account "TheAccount".

If other form of credentials are used (jwt, nkey or other), then the server will attempt to authenticate and if successful associate to the account for that specific user. If the user authentication fails (wrong password, no such user, etc..) the connection will be also rejected.


## LeafNode `remotes` Entry Block

| Property | Description |
| :------  | :---- |
| `url` | Leafnode URL (URL protocol should be `nats-leaf`). |
| `urls` | Leafnode URL array. Supports multiple URLs for discovery, e.g., urls: [ "nats-leaf://host1:7422", "nats-leaf://host2:7422" ]|
| `account` | Account public key identifying the leafnode. Account must be defined locally. |
| `credentials` | Credential file for connecting to the leafnode server. |
| `tls` | A TLS configuration block. Leafnode client will use specified TLS certificates when connecting/authenticating. |

## `tls` Configuration Block

| Property | Description |
| :------  | :---- |
| `cert_file` | TLS certificate file. |
| `key_file` | TLS certificate key file. |
| `ca_file` | TLS certificate authority file. |
| `insecure` | Skip certificate verification. |
| `verify` | If `true`, require and verify client certificates. |
| `verify_and_map` | If `true`, require and verify client certificates and use values map certificate values for authentication purposes. |
| `cipher_suites` | When set, only the specified TLS cipher suites will be allowed. Values must match golang version used to build the server.  |
| `curve_preferences` | List of TLS cypher curves to use in order. |
| `timeout` | TLS handshake timeout in fractional seconds. |


