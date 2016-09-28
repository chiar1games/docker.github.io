---
title: "Getting Started, Part 3: Stateful, Multi-container Applications"
---

# Getting Started, Part 3: Stateful, Multi-container Applications

In [Getting Started, Part 2: Creating and Building Your App](part2.md), we
wrote, built, ran, and shared our first Dockerized app, which all fit in a
single container.

In part 3, we will expand this application so that it is comprised of two
containers simultaneously: one running the web app we have already written, and
another that stores data on the web app's behalf.

## Understanding services

In a world where every executable is running in a container, things are very
fluid and portable, which is exciting. There's just one problem: if you run
two containers at the same time, they don't know about each other. Each
container is isolated from the host environment, by design -- that's how Docker
enables environment-agnostic deployment.

We need something that defines some connective tissue between containers, so
that they run at the same time, and have the right ports open
so they can talk to each other. It's obvious why: having a front-end application
is all well and good, but it's going to need to store data at some point,
and that's going to happen via a different executable entirely.

In a distributed application, these different pieces of the app are called
"services." For example, if you imagine a video sharing site, there will
probably be a service for storing application data in a database, another one
for video transcoding in the background after a user uploads something, a
service for streaming, and so on, and they all need to work in concert.

The easiest way to introduce the organization of your app into services using
containers is by using Docker Compose. We're going to add a data storage service
to our simple Hello World app. Don't worry, it's shockingly easy.

## Your first `docker-compose.yml` File

As you saw, a `Dockerfile` is a flat text file full of directives that define
what goes in a single Docker image. But a `docker-compose.yml` file is a YAML
markup file that is hierarchical in structure, and defines how multiple Docker
images should work together when they are running in containers.

We saw that the "Hello World" app we created looked for a running instance of
Redis, and if it failed, it produced an error message. Well, just as we grabbed
the base image of Python so we could have a runtime for our app, we can grab the
official image of Redis, and run that right alongside the app, so the check will
pass.

Save this `docker-compose.yml` file:

{% gist johndmulhausen/7b8e955ccc939d9cef83a015e06ed8e7 %}

Yes, that's all you need to specify, and Redis will be pulled and run. When
running out-of-the-box software that your app depends on (like Redis, or MySQL),
no `Dockerfile` is necessary, unless you want to make a custom image that has
all your preferences "baked in." In this case, we're just using the official
Redis image and accepting the default settings they use. (Redis documents these
defaults on [the page for the official Redis
image](https://store.docker.com/images/1f6ef28b-3e48-4da1-b838-5bd8710a2053)).

To break down what this `docker-compose.yml` file is specifying:

- Pull and run [the image we uploaded in step 2](/getting-started/part2/#/share-the-app) as a service called `web`
- Map port 80 on the host machine running this container to port 80 inside the container (so http://localhost:80 resolves properly)
- Link this container to the service we named `redis`; this ensures that the
  dependency between `redis` and `web` is expressed, as well as the order of service
  startup.
- Pull and run the official Redis image as a service called `redis`

## Run your first multi-container app

Run this command in the same directory where you saved `docker-compose.yml`:

```shell
docker-compose up
```

This will pull all the necessary images and run them in concert. Now when you
visit `http://localhost`, you'll see a number next to the visitor counter
instead of the error message. It really works -- just keep hitting refresh.

## Yes, that's really Redis

With a containerized instance of Redis running, you're probably wondering --
how do I break through the wall of isolation and manage my data? The answer is,
port mapping. [The page for the official Redis
image](https://store.docker.com/images/1f6ef28b-3e48-4da1-b838-5bd8710a2053)
states that the normal management ports are open in their image, so you should
be able to connect to it at `localhost:6379` if you add a `ports:` section to
`docker-compose.yaml` under `redis` that maps `6379` to your host, just as port
`80` is mapped for `web`. Same with MySQL or any other data solution;
containerized doesn't mean unreachable, it just means portable. Once you map
your ports, you can use your fave UI tools like MySQL Workbench, Redis Desktop
Manager, etc, to connect to your Dockerized instance.

[On to next >>](part4.md){: class="button darkblue-btn"}
