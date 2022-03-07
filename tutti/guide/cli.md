# Tutti CLI Commands

Tutti provides a command line interface that allows Tutti.works server maintainers to run operations for service daemon.
The command can be executed by starting with `./tutti` which only available from Tutti.works' root directory.

## Usage example

```
cd /path/to/tutti
```

```
# Display help
./tutti 

# Build Docker images for Tutti.works service
./tutti build

# Start Tutti.works server
./tutti start

# Stop Tutti.works server
./tutti stop
```

## Commands

##### `./tutti init`

Initializes configurations for Tutti.works' Docker Compose environment (needed only for Linux).
This command should run only once.
Specifically, this command covers the following:

- Creates "docker" user group if it does not exist
- Adds current user to docker group
- Appends the following in `~/.bashrc` for Docker's BuildKit settings
    ```
    # Tutti envs
    export HOST_WEBAPI_ADDRESS=172.0.0.1
    export DOCKER_BUILDKIT=1
    export COMPOSE_DOCKER_CLI_BUILD=1
    ```
- Launch new terminal session with `newgrp` Linux command

##### `./tutti build`

Builds Docker images for Tutti.works environment. Equivalent to `docker-compose build`.

##### `./tutti start`

(Re)create all Docker containers for Tutti.works environment (Runs `docker-compose up -d`).
This also starts a web server (for project rebuilding purpose) in the host machine environment.

##### `./tutti stop`

Stops all Docker containers for Tutti works envrionment (Runs `docker-compose down`).
This also stops a web server (for project rebuilding purpose) in the host machine environment.

##### `./tutti restart-host`

Restarts a web server (for project rebuilding purpose) in the host machine environment.

##### `./tutti kill`

Forces Tutti.works environment to stop immediately.
Equivalent to `docker-compose kill` and killing a process for the web server for project rebuild.

!> This operation may lose some recent data in Redis that has not yet been dumped to file.

##### `./tutti log <service_name>`

Prints and forwards log of a specified Docker container (Runs `docker-compose logs -f --tail="200" <service_name>`).
Valid service names are all entries that are printed with `docker-compose config --services` PLUS "host" (project rebuild server).
