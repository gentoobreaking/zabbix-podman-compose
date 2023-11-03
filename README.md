# Podman Nonsupport items:

## Error: unsupported bridge network option com.docker.network.enable_ipv6

    driver_opts:
      com.docker.network.enable_ipv6: "false"

## Error: statfs /etc/timezone: no such file or directory

   - /etc/timezone:/etc/timezone:ro

## Error: opening seccomp profile failed: open ./env_vars/chrome_dp.json: no such file or directory

   security_opt:
    - seccomp:./env_vars/chrome_dp.json

## Error: --network-alias can only be set when the network mode is bridge: invalid argument

  networks:
   zbx_net_backend:
    aliases:
     - zabbix-web-service
     - zabbix-web-service-alpine

## Error: crun: setrlimit `RLIMIT_NPROC`: Operation not permitted: OCI permission denied
  exit code: 126

  ulimits:
#   nproc: 65535
    nofile:
    soft: 20000
    hard: 40000

## Error: rootlessport cannot expose privileged port 80, you can add 'net.ipv4.ip_unprivileged_port_start=80' to /etc/sysctl.conf (currently 1024), or choose a larger port number (>= 1024): listen tcp 0.0.0.0:80: bind: permission denied
  exit code: 126
  podman start zabbix-web-nginx-mysql
  Error: unable to start container "fdb3cc376f0181b88dadfe6ede08b9d3bba7b65aa436f1ba3f6418b68756fef4": rootlessport cannot expose privileged port 80, you can add 'net.ipv4.ip_unprivileged_port_start=80' to /etc/sysctl.conf (currently 1024), or choose a larger port number (>= 1024): listen tcp 0.0.0.0:80: bind: permission denied
  exit code: 125

-> 80->8080 , 443->8443
 zabbix-web-nginx-mysql:
  container_name: zabbix-web-nginx-mysql
  image: zabbix/zabbix-web-nginx-mysql:alpine-6.4-latest
  ports:
   - "8080:8080"
   - "8443:8443"

## mysql volume mount local.

# $ podman logs -f mysql-server
2023-11-03 03:33:11+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.35-1.el8 started.
chown: changing ownership of '/var/lib/mysql/': Operation not permitted
chown: changing ownership of '/var/lib/mysql': Operation not permitted

=>
  mysql-server:
  container_name: mysql-server
  image: mysql:8.0-oracle
  userns_mode: 501

## Error: unrecognized namespace mode 501 passed


----

podman-compose -f docker-compose.yaml up -d

## Login Web Gui:

This is the Zabbix welcome screen. Enter the user name Admin with password zabbix to log in as a Zabbix superuser. Access to all menu sections will be granted.

