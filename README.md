# Clair-POC
POC for clair (https://github.com/coreos/clair) + clairctl (https://github.com/jgsqware/clairctl)

# Installation

## Docker Distribution (ex Docker Registry) setup
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
Then you can either use docker-compose way or run each container separately or even use Kubernetes

## Docker-compose

```bash
$ curl -L https://raw.githubusercontent.com/coreos/clair/master/docker-compose.yml -o $HOME/docker-compose.yml
$ mkdir $HOME/clair_config
$ curl -L https://raw.githubusercontent.com/coreos/clair/master/config.example.yaml -o $HOME/clair_config/config.yaml
$ $EDITOR $HOME/clair_config/config.yaml # Edit database source to be postgresql://postgres:password@postgres:5432?sslmode=disable
$ docker-compose -f $HOME/docker-compose.yml up -d
```
Docker Compose may start Clair before Postgres which will raise an error. If this error is raised, manually execute docker-compose start clair.

## Docker

```bash
$ mkdir $PWD/clair_config
$ curl -L https://raw.githubusercontent.com/coreos/clair/master/config.example.yaml -o $PWD/clair_config/config.yaml
$ docker run -d -e POSTGRES_PASSWORD="" -p 5432:5432 postgres:9.6
$ docker run -d -p 6060-6061:6060-6061 -v $PWD/clair_config:/config quay.io/coreos/clair-git:latest config=/config/config.yaml
```

## Kubernetes

If you don't have a local Kubernetes cluster already, check out [minikube](https://github.com/kubernetes/minikube).
```bash
$ git clone https://github.com/coreos/clair
$ cd clair/contrib/k8s
$ kubectl create secret generic clairsecret --from-file=./config.yaml
$ kubectl create -f clair-kubernetes.yaml
```
