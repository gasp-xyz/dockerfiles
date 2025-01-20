# github-actions-runner to be used in github actions workflow self-hosted runners

```bash
# build and push latest image
docker buildx build -t gaspxyz/github-actions-runner:latest --platform linux/amd64,linux/arm64 --push .
```
