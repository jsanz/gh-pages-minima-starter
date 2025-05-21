---
title: "Tutorial VI: local environment"
date: 2020-4-17
layout: post
---

I've included a simple `docker-compose.yml` file in the root of the repo, so you can spin up a local environment without having to install in your computer Ruby or any other library. If you are versed in `Ruby`, `gems`, and `bundle` you can probably skip this section and go ahead!

Just install [Docker][docker] and [Docker Compose][compose], visit the repo folder and run `docker compose build`. Then, every time you wan to spin up the local environment just run `docker compose up` and visit `http://localhost:8080/gh-pages-minima-starter`. 

Alternatively, if you prefer to install all the software locally please follow the [docs][install].

The compose file is super simple, it just refers to a local `Dockerfile` image definition setting up the current folder as the container `/srv/jekyll` directory and starts the server with convenient options like livereload.

```yaml
version: '3'

services:
  jekyll:
    build:
      context: .
    command: jekyll serve --watch --force_polling --incremental --port 8080 -H 0.0.0.0 --livereload --livereload-port 35729 --baseurl /gh-pages-minima-starter
    volumes:
      - .:/srv/jekyll
    ports:
      - '8080:8080'
      - '35729:35729'
    environment: 
      RUBYOPT: '-W0'
      JEKYLL_ENV: production 
```

[install]: https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll
[docker]: https://docs.docker.com/get-docker/
[compose]: https://docs.docker.com/compose/
