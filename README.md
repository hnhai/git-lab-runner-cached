# Gitlab-runner with cache
gitlab-runner \
  register -n \
  --name "git-lab-runner-name" \
  --executor docker \
  --docker-image ubuntu-image \
  --docker-volumes /var/run/docker.sock:/var/run/docker.sock \
  --url https://git-lab-url.com.vn/ \
  --registration-token "$token" \
  --tag-list "$tags" \
  --cache-type s3 \
  --cache-s3-server-address "${ip}:9000/" \
  --cache-s3-access-key "minioadmin" \
  --cache-s3-secret-key "minioadmin" \
  --cache-s3-bucket-name "runner" \
  --cache-s3-insecure "true"

# Docker-compose file
`
services:
   minio:
     network_mode: host
     image: minio/minio:latest
     restart: always
     volumes:
       - ./minio:/export
       - ./minio_config:/root/.minio
     entrypoint: sh
     command: -c 'mkdir -p /export/runner && /usr/bin/minio server /export'
   gitlab-runner:
     container_name: gitlab-runner
     depends_on:
       - minio
     image: gitlab/gitlab-runner:latest
     volumes:
       - ./gitlab-runner:/etc/gitlab-runner
       - /var/run/docker.sock:/var/run/docker.sock
     restart: always
`
