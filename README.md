# Tornado systemd socket activation support

This little library allows easy systemd socket activation support for tornado.

## How to use

First you have to install your systemd service and socket files:

Example:

`/etc/systemd/system/butterfly.service`
```ini
[Unit]
Description=Butterfly Terminal Server

[Service]
ExecStart=/usr/bin/butterfly.server.py
```

`/etc/systemd/system/butterfly.socket`
```ini
[Socket]
ListenStream=57575

[Install]
WantedBy=sockets.target
```

Then it's as simple as:

```python
from tornado_systemd import SystemdHTTPServer
http_server = SystemdHTTPServer(application)
http_server.listen(port, address=host)
```

instead of

```python
from tornado.web import HTTPServer
http_server = HTTPServer(application)
http_server.listen(port, address=host)
```

or the simpler

```python
application.listen(port)
```

## Nota Bene

This wrapper checks if the service has been launched by systemd socket
activation.
This means it still works when run by regular ways.

The server and the application get a `systemd` attribute which are `True` if
the server was activated with systemd

Example:

```python
    if self.application.systemd and not len(TermWebSocket.terminals):
        self.log.info('No more terminals, exiting...')
        sys.exit(0)
```
