# Elixir release in a container

https://www.erlang.org/doc/man/kernel_app.html

https://elixir-lang.org/getting-started/mix-otp/config-and-releases.html#vm-args

https://www.erlang-solutions.com/blog/erlang-and-elixir-distribution-without-epmd/

## Expose the ports of a distributed Erlang node.

First set kernel configuration parameters limit the port range for the listener socket of a distributed Erlang node.

in `mix-project/rel/vm.args.eex` add:

```
-kernel inet_dist_listen_min 9000
-kernel inet_dist_listen_max 9010
```

## Export ports on continer

In addition to the above you have to expose port 4369 for epmd, the Erlang Port Mapper Daemon.

```
$ podman run -d -p 4369:4369 -p 9000-9010:9000-9010 <image>
```
