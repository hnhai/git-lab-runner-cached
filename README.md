# Gitlab-runner with cache
gitlab-runner \
  register -n \
  --name "git-lab-runner-name" \
  --executor docker \
  --docker-image hacbq/docker:latest \
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

