---
title: Production Setup
---

## Setup https

You can setup a reverse proxy using [Caddy](https://caddyserver.com/docs/quick-starts/reverse-proxy).

## Make sure [--host](/docs/reference/command-line#--host-string), [--port](/docs/reference/command-line#--port-number) match exactly to the host:port address where Bytebase supposed to be visited.

Bytebase uses --host, --port to configure the VCS webhook callback used by the [project version control workflow](/docs/vcs-integration/enable-version-control-workflow#step-3-configure-deploy). If host:port mismatches, then committed migration scripts will not trigger the issue creation in Bytebase.

If Bytebase is run by Docker, make sure --publish `hostport`:`containerport` and the `--port` flag be the same.

Like the example below, all 3 ports are 5678: --publish **5678**:**5678** --port **5678**

`docker run --init --name bytebase --restart always --publish 5678:5678 --volume ~/.bytebase/data:/var/opt/bytebase bytebase/bytebase:<<version>> --data /var/opt/bytebase --host http://localhost --port 5678`

## Kubernetes

### Use Persistent Volume

If Bytebase is configured to store either metadata or the backups on the local disk, then you must use [Persistent Volume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes). Local disk cannot persist state and can also cause frequent pod eviction due to disk pressure during backup.

```
Status: Failed
Reason: Evicted
Message: Pod The node had condition: [DiskPressure].
```
