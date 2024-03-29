## `LeafNode` Configuration Block

| Property | Description |
| :------  | :---- |
| `advertise` | Hostport `<host>:<port>` to advertise to other gateways. |
| `authorization` | Authorization block (same as other nats-server `authorization` configuration). |
| `host` | Interface where the gateway will listen for incomming gateway connections. |
| `listen` | Combines `host` and `port` as `<host>:<port>` |
| `name` | Name for this cluster, all gateways belonging to the same cluster, should specify the same name. |
| `no_advertise` | if `true` the leafnode shouldn't be advertised. |
| `port` | Port where the gateway will listen for incomming gateway connections. |
| `remotes` | List of `remote` entries specifying servers where leafnode client connection can be made. |
| `tls` | TLS configuration block (same as other nats-server `tls` configuration). |


### LeafNode `Remote` Entry Block

| Property | Description |
| :------  | :---- |
| `url` | Leafnode URL (URL protocol should be `leafnode`). |
| `account` | Account public key identifying the leafnode. Account must be defined locally. |
| `credentials` | Credential file for connecting to the leafnode server. |
| `tls` | A TLS configuration block. Gateway client will use specified TLS certificates when connecting/authenticating. |

### `TLS` Configuration Block

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


