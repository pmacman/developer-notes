# Docker Cheat Sheet

## Check Docker Status

**Running containers:**

```bash
docker ps
```

**All containers (running + stopped):**

```bash
docker ps -a
```

**Status of this app:**

```bash
docker compose ps
```

## Stop a Docker Container

```bash
docker compose down
```

## Start a Docker Container

```bash
docker compose up -d
```

## Restart a Docker Container

**Quick restart:**

```bash
docker compose restart
```

**Full cycle:**

```bash
docker compose down
docker compose up -d
```

## Update App to Latest Version

```bash
docker compose pull
docker compose up -d
```

**Optional cleanup:**

```bash
docker image prune -f
```

## Uninstall / Remove App

Stop containers:

```bash
docker compose down
```

Remove data volumes (**destructive**):

```bash
docker compose down -v
```

## Useful Global Docker Commands

Logs:

```bash
docker compose logs -f
```

Disk usage:

```bash
docker system df
```

Safe cleanup:

```bash
docker system prune
```

Aggressive cleanup:

```bash
docker system prune -a
```

## Summary Cheat Sheet

```bash
docker compose ps         # status
docker compose up -d      # start (uses image already installed)
docker compose down       # stop
docker compose restart    # restart
docker compose pull       # update image
docker compose up -d      # apply update
docker compose down -v    # uninstall + delete data

# start (pulls latest image)
docker compose up -d --pull always
# This is the equivalent of running:
# docker compose pull
# docker compose up -d

docker logs <container>           # view logs
docker logs -f <container>        # follow logs
docker exec -it <container> bash  # shell into container
```