# example-wsgi-app-circus

A demonstration of Python deployment with WSGI using [Circus](https://circus.readthedocs.io/en/latest/).

> Circus is a Process & Socket Manager

---

## Installation

Here, a tutorial step by step of deployment with Plone WSGI using Circus:

### Pre dependencies

Create a virtual environment, executing the following command:

```bash
$ python3 -m venv ./venv
```

Activate the virtual environment, executing the following command:

```bash
$ source ./venv/bin/activate
```

Install the pre-requirements Python dependencies, executing the following command:

```bash
$ ./venv/bin/pip3 install -r requirements.txt
```

Run Hello World app, executing the following command:

```bash
$ ./venv/bin/chaussette
```

You should be able to visit [http://localhost:8080](http://localhost:8080) and see **hello world** message.

Stop ``Chaussette`` and add a ``circus.ini`` file for the circus daemon service in the directory containing:

```
[circus]
statsd = True

[watcher:webapp]
cmd = venv/bin/chaussette --fd $(circus.sockets.web)
numprocesses = 3
use_sockets = True

[socket:web]
host = 127.0.0.1
port = 9999
```

Save the changes and close the ``circus.ini`` file.

---

## Circus scripts

Circus installs some scripts to manage this project.

### circus daemon

Run it using ``circusd``, executing the following command:

```bash
$ ./venv/bin/circusd ./circus.ini
```

Now visit [http://127.0.0.1:9999](http://127.0.0.1:9999), you should  see the **hello world** app. The difference now is that the socket is managed by Circus and there are several web workers that are accepting connections against it.

Also can run as a daemon service, executing the following command:

```bash
$ ./venv/bin/circusd --daemon ./circus.ini
```

This keeps ``circusd`` in the foreground mode.

### circus top

You can checkout the monitor stats console, like a ``top`` command, executing the following command:

```bash
$ ./venv/bin/circus-top
```

### circus control

You can checkout the status service, executing the following command:

```bash
$ ./venv/bin/circusctl status
```

You can start the circus service, executing the following command:

```bash
$ ./venv/bin/circusctl start
```

You can stop the circus service, executing the following command:

```bash
$ ./venv/bin/circusctl stop
```

You can restart the circus service, executing the following command:

```bash
$ ./venv/bin/circusctl restart
```

Full Documentation is available at [https://circus.readthedocs.io/](https://circus.readthedocs.io/).
