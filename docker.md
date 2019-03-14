### Allow user nonroot run docker command (doesn't require a restart)
Run:
  sudo setfacl -m user:username:rw /var/run/docker.sock
