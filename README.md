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

## Not support sysctl under RHEL with podman-compose rootless mode. But Mac doesn't.

['podman', 'network', 'exists', 'zabbix-podman-compose_zbx_net_frontend']
podman run --name=zabbix-web-nginx-mysql -d --requires=mysql-server,zabbix-server --label com.zabbix.description=Zabbix frontend on Nginx web-server with MySQL database support --label com.zabbix.company=Zabbix LLC --label com.zabbix.component=zabbix-frontend --label com.zabbix.webserver=nginx --label com.zabbix.dbtype=mysql --label com.zabbix.os=alpine --label io.podman.compose.config-hash=f1532168f0608292950dbb0bb26cc15e48dc3ef5277d01433e6f582e1a5376d2 --label io.podman.compose.project=zabbix-podman-compose --label io.podman.compose.version=1.0.6 --label PODMAN_SYSTEMD_UNIT=podman-compose@zabbix-podman-compose.service --label com.docker.compose.project=zabbix-podman-compose --label com.docker.compose.project.working_dir=/home/middcadm/zabbix-podman-compose --label com.docker.compose.project.config_files=docker-compose.yaml --label com.docker.compose.container-number=1 --label com.docker.compose.service=zabbix-web-nginx-mysql --env-file /home/middcadm/zabbix-podman-compose/env_vars/.env_db_mysql --env-file /home/middcadm/zabbix-podman-compose/env_vars/.env_web -v /etc/localtime:/etc/localtime:ro -v /home/middcadm/zabbix-podman-compose/zbx_env/etc/ssl/nginx:/etc/ssl/nginx:ro -v /home/middcadm/zabbix-podman-compose/zbx_env/usr/share/zabbix/modules:/usr/share/zabbix/modules/:ro --net zabbix-podman-compose_zbx_net_frontend,zabbix-podman-compose_zbx_net_backend --network-alias zabbix-web-nginx-mysql --volume /home/middcadm/zabbix-podman-compose/env_vars/.MYSQL_USER:/run/secrets/MYSQL_USER:ro,rprivate,rbind --volume /home/middcadm/zabbix-podman-compose/env_vars/.MYSQL_PASSWORD:/run/secrets/MYSQL_PASSWORD:ro,rprivate,rbind -p 8080:8080 -p 8443:8443 --sysctl net.core.somaxconn=65535 --cpus 0.7 -m 512m --memory-reservation 256m --healthcheck-command /bin/sh -c curl' '-f' 'http://localhost:8080/ping --healthcheck-interval 10s --healthcheck-timeout 5s --healthcheck-start-period 30s --healthcheck-retries 3 localhost/zabbix/zabbix-web-nginx-mysql:alpine-6.4.7
Resource limits are not supported and ignored on cgroups V1 rootless systems
WARN[0001] Failed to mount subscriptions, skipping entry in /usr/share/containers/mounts.conf: getting host subscription data: failed to read subscriptions from "/usr/share/rhel/secrets": open /usr/share/rhel/secrets/redhat.repo: permission denied
Error: runc: runc create failed: unable to start container process: error during container init: open /proc/sys/net/core/somaxconn: no such file or directory: OCI runtime attempted to invoke a command that was not found
exit code: 127
podman start zabbix-web-nginx-mysql
Error: unable to start container "66bc7ffe601b1d1ebb101e2b047947de773820d72f2a2b811a0a491d307d087f": runc: runc create failed: unable to start container process: error during container init: open /proc/sys/net/core/somaxconn: no such file or directory: OCI runtime attempted to invoke a command that was not found
exit code: 125

  sysctls:
   - net.core.somaxconn=65535

----

podman-compose -f docker-compose.yaml up -d

## Login Web Gui:

This is the Zabbix welcome screen. Enter the user name Admin with password zabbix to log in as a Zabbix superuser. Access to all menu sections will be granted.

