# Clair-POC
POC for clair (https://github.com/coreos/clair) + clairctl (https://github.com/jgsqware/clairctl)

# Installation

## Docker Distribution (ex Docker Registry)
Run the local copy of Docker Registry (Distributuon)
```bash
$ docker run -d -p 5000:5000 --name registry registry:2
```
If you want to use insecure registry don't forget to add `/etc/docker/daemon.json` file with following content
```json
{
    "insecure-registries" : [ "localhost:5000" ]
}
```
and restart Docker daemon

```bash
$ systemctl daemon-reload
$ systemclt restart docker
```

## Docker-compose


```bash
$ curl -L https://raw.githubusercontent.com/coreos/clair/master/docker-compose.yml -o $HOME/docker-compose.yml
$ mkdir $HOME/clair_config
$ curl -L https://raw.githubusercontent.com/coreos/clair/master/config.example.yaml -o $HOME/clair_config/config.yaml
$ $EDITOR $HOME/clair_config/config.yaml # Edit database source to be postgresql://postgres:password@postgres:5432?sslmode=disable
$ docker-compose -f $HOME/docker-compose.yml up -d
```

