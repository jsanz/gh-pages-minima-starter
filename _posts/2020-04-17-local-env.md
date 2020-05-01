---
title: "Tutorial VI: local environment"
date: 2020-4-17
layout: post
---

I've included a simple `docker-compose.yml` file in the root of the repo, so you can spin up a local environment without having to install in your computer Ruby or any other library. Just install [Docker][docker] and [Docker Compose][compose], visit the repo folder and run `docker-compose up` and visit `http://localhost:8080`. 

Alternatively, if you prefer to install all the software locally please follow the [docs][install].

The compose file is super simple, it just refers to Bret Fisher Jekyll image setting up the current folder as the container `/site` directory and saves in a host volume the jekyll files to avoid having to download them every time you start the container.

```yaml
version: '3'

services:
  jekyll:
    image: bretfisher/jekyll-serve
    volumes:
      - .:/site
      - jekyll:/usr/local/bundle
    ports:
      - '4000:4000'

volumes:
    jekyll:

```

[install]: https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll
[docker]: https://docs.docker.com/get-docker/
[compose]: https://docs.docker.com/compose/install/
