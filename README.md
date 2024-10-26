Librechat is a feature-rich, self-hostable interface to image and text **generative AI** models.

This repo contains some modifications I did to get it running on my host. This may not be ideal per the official docs.

## Setup notes

* `docker login` to `ghcr.io` using a github token.

* Create directories used by librechat in the cloned folder. (If compose creates it, these directories seem to be owned by root, which causes problem.)

    ```bash
    mkdir -p logs images
    ```

* Run docker compose

    ```bash
    UID=$(id -u) GID=$(id -g) PORT=3080 docker compose up -d
    ```

* Create a user using the interactive command

    ```bash
    docker compose exec api npm run create-user
    ```

* Login to `localhost:$PORT` via browser.

* To shut down, simply run `docker compose down` in the same directory.

## Config file for a local ollama

```yaml
version: 1.1.4
cache: true
endpoints:
  custom:
    - name: "Ollama"
      apiKey: "ollama"
      baseURL: "http://host.docker.internal:11434/v1"
      models:
        default: [
          "llama3.2:latest",
          ]
        fetch: true # fetching list of models is not supported
      titleConvo: true
      titleModel: "LLaMa 3.2 (3B OLLaMa)"
```

## TODO: ollama multiple models
## TODO: stable diffusion

Refer to [upstream documentation](https://www.librechat.ai/docs) for more detail.

## Notes:
* firewall can give problem connecting to ollama (localhost) from librechat (docker network). Appropriate firewall tweaks to allow ingres from docker network on docker0 interface.
