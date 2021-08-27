---
Title: Github Docker Registry
thumbnailImagePosition: left
thumbnailImage: https://github.githubassets.com/images/modules/site/social-cards/package-registry.png
date: 2021-08-17T00:00:00.000Z
metaAlignment: center
coverMeta: out
categories: [Software]
tags: [github, container, docker]
---

{{< toc >}}

---
## Account Preparation

1.  Create a Repo [here](https://github.com/new)
2.  Create a new personal access token (PAT) with the appropriate scopes for the tasks you want to accomplish. If your organization requires SSO, you must enable SSO for your new token. [Create Here](https://github.com/settings/tokens/new?scopes=write:packages,delete:packages)
3.  Using the CLI for your container type, sign in to the Container registry service at ghcr.io.<br>
    ```shell
    echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin
    ```
4.  Save your PAT. We recommend saving your PAT as an environment variable.<br>
    ```shell
    export CR_PAT=YOUR_TOKEN
    ```

## Push to Container registry

### Attached Packages to Account

1.  Write your Docekrfile
2.  Building container images<br>
    ```shell
    $ docker build -t hello_docker .
    ```
3.  Tagging container images
    1.  Find the ID for the Docker image you want to tag.
        ```shell
        $ docker images
        > REPOSITORY                                            TAG                 IMAGE ID            CREATED             SIZE
        > ghcr.io/my-org/hello_docker         latest              38f737a91f39        47 hours ago        91.7MB
        > ghcr.io/my-username/hello_docker    latest              38f737a91f39        47 hours ago        91.7MB
        > hello-world                                           latest              fce289e99eb9        16 months ago       1.84kB
        ```
    2.  Tag your Docker image using the image ID and your desired image name and hosting destination.
        ```shell
        docker tag 38f737a91f39 ghcr.io/OWNER/NEW_IMAGE_NAME:latest
        ```
    3.  Pushing container images
        ```shell
        $ docker push ghcr.io/OWNER/IMAGE_NAME:latest
        ```

### Attached Packages to a Repo

1.  Write your Docekrfile

    ```ad-warning
    title:  Label your Docker Image
    collapse:    open

    	LABEL org.opencontainers.image.source="https://github.com/OWNER/REPO_NAME"
    ```
2.  Building container images<br>
    ```shell
    docker build -t ghcr.io/OWNER/REPO_NAME/IMAGE_NAME:latest .
    ```
3.  Tagging container images
    1.  Find the ID for the Docker image you want to tag.
        ```shell
        $ docker images
        > REPOSITORY                                            TAG                 IMAGE ID            CREATED             SIZE
        > ghcr.io/my-org/hello_docker         latest              38f737a91f39        47 hours ago        91.7MB
        > ghcr.io/my-username/hello_docker    latest              38f737a91f39        47 hours ago        91.7MB
        > hello-world                                           latest              fce289e99eb9        16 months ago       1.84kB
        ```
    2.  Tag your Docker image using the image ID and your desired image name and hosting destination.
        ```shell
        docker tag 38f737a91f39 ghcr.io/OWNER/NEW_IMAGE_NAME:latest
        ```
    3.  Pushing container images
        ```shell
        $ docker push ghcr.io/OWNER/IMAGE_NAME:latest
        ```

## Security

1.  Access to this package can be managed via the packages settings
    1.  Can be inherited from the repo's ACL  or can be specified separately
