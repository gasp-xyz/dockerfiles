# github-actions-runner to be used in github actions workflow self-hosted runners

```bash
# build and push latest image
docker buildx build -t gaspxyz/github-actions-runner:2.324.0 --platform linux/amd64,linux/arm64 --push .
```

Latest version here: <https://github.com/actions/runner/releases>
