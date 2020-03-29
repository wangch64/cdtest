# docker-in-github-actions-vs-drone

docker build in GitHub Actions vs Drone CI/CD

[![Build Status](https://cloud.drone.io/api/badges/go-training/docker-in-github-actions-vs-drone/status.svg)](https://cloud.drone.io/go-training/docker-in-github-actions-vs-drone)
![CI](https://github.com/go-training/docker-in-github-actions-vs-drone/workflows/CI/badge.svg)

## GitHub Actions

```yml
- name: build and push image
  uses: docker/build-push-action@v1
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}
    repository: appleboy/gin-docker-demo
    dockerfile: Dockerfile
    always_pull: true
    tags: latest
```

## Drone CI

```yml
- name: publish
  pull: always
  image: plugins/docker:linux-amd64
  settings:
    auto_tag: true
    cache_from: appleboy/gin-docker-demo
    daemon_off: false
    dockerfile: Dockerfile
    password:
      from_secret: docker_password
    repo: appleboy/gin-docker-demo
    username:
      from_secret: docker_username
  when:
    event:
      exclude:
      - pull_request
```

## Sumary

build time

* GitHub Actions: 2m2s
* Drone CI: **17s**
