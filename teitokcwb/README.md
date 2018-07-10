# Dockerfile to get [TEITOK](http://teitok.corpuswiki.org/site/) and Dependencies up'n running

Get (and compile) dependencies for TEITOK, and install and configure a simple TEITOK site.


## Quick Start

*Important:* you need access to `https://gitlab.com/maartenes/TEITOK` first.

1. Install docker

    Go to https://docs.docker.com/engine/installation/ and install the
    appropriate Docker CE for your system.

1. Build and start a container

```
git co https://gitlab.inf.unibz.it/commul/docker/teitok.git
cd teitok
git co https://gitlab.com/maartenes/TEITOK
docker build -t teitok:latest -f Dockerfile .
docker run teitok:latest
```

you will see something like: `...using 172.17.0.2.` and then you should be able to access:

[`http://172.17.0.2/teitok/demo/`](http://172.17.0.2/teitok/demo/)


## TODOs

- [ ] Configure simple site
- [ ] Configure apache
- [ ] ...

## References

 - http://teitok.corpuswiki.org/site/index.php?action=help
