# docker-varnishd
A Docker container for running Varnish Reverse Proxy

This container can be used to run a [varnishd](https://varnish-cache.org/) reverse proxy instance. The container can be used as-is, or extended to customize configuration, versions, etc.

This container is based on the CentOS 7 Docker image, and uses YUM for package management. The Varnishd software package is installed from the EPEL repository.

## Building

```sh
docker build . --build-arg VERSION=4.0.5 -t varnishd
```

## Usage
To run the container using the default settings and VCL:
```sh
docker run -p 80:80 limit0/varnishd
```

To customize the Varnish configuration (VCL), either extend this image and `ADD` your configuration, or mount the `/etc/varnish` directory and override the `default.vcl`. *Note: You will also have to specify the `secret` and `varnish.params` if volume mounting -- see the default examples in the `/etc` folder of this repository.

### Logging
`varnishd` does not log requests to `stdout`/`stderr`, but rather uses its own `varnishlog` application to access logs from the running process. You can easily access varnish logs by including the `name` parameter when running the container, and running a second process to view the logs using `varnishlog`.
```sh
docker run -p 80:80 --name varnishd limit0/varnishd
docker exec -it varnishd varnishlog
```

You can utilize the same named container access method to use other varnish administrative and logging tools, such as `varnishadm`, `varnishhist`, `varnishncsa`, `varnishstat`, `varnishtop`, `varnishlog`, `varnish_reload_vcl`, or `varnishtest`.
