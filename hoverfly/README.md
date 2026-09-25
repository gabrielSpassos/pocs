<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Hoverfly](#hoverfly)
    - [What is it?](#what-is-it)
    - [Flavors](#flavors)
    - [Local Install](#local-install)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Hoverfly

> https://github.com/spectolabs/hoverfly

### What is it?

* lightweight, open source API simulation tool
* Replace slow, flaky API dependencies with realistic, re-usable simulations
* Simulate latency
* Simulate random errors
* Simulate rate limit
* Written in GO

### Flavors

* local
* cloud - Hoverfly Cloud - API simulations as a service

### Local Install

> https://docs.hoverfly.io/en/latest/pages/introduction/downloadinstallation.html

* Extract the .zip and place the binaries `hoverctl` and `hoverfly` into `.local/bin
`

* Add location to your `PATH` at `~/.bashrc` or `~/.zshrc`
```bash
export PATH="$HOME/.local/bin:$PATH"
```

* Test installation
```bash
source ~/.zshrc 
hoverctl version
hoverfly -version
```

* Start hoverfly
```bash
hoverctl start
```

* Check hoverfly logs
```bash
hoverctl logs
```

* Stop hoverfly 
```bash
hoverctl stop
```

* Admin dashboard: http://localhost:8888/dashboard

* [Hoverfly as a proxy server](proxy-server.md)

* [Hoverfly as a web server](web-server.md)

* [Hoverfly modes](modes.md)

* [Simulations](simulations.md)

* Install Via Docker

```bash
docker run -d -p 8888:8888 -p 8500:8500 spectolabs/hoverfly:latest
```

//TODO
* Configure the downloaded cli tool `hoverctl` to use the docker container

