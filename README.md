Nextcloud: a safe home for all your data
========================================

Motivation
--

My initial goal when starting this project was to move all my sensitive data from Google Drive to a self-hosted Nextcloud instance.

Deploying Nextcloud on Docker was an evidence since I already had some experience with it, I though it would be rather straightforward to deploy a nextcloud instance with a reverse proxy and HTTPS support to securely access my data everywhere in a secure way.

But it was not that easy, mainly because of permission issues regarding shared volumes between the fpm and the nginx container and the examples provided were not satisfying because I intended to deploy Nextcloud on a swarm stack.

The system is now stable and can survive a reboot.

Installation
------------

1. Ensure that you have docker configured in swarm mode.

    ```
    docker swarm init
    ```
    on the master node.
2. Ensure that you have Traefik configured as a reverse proxy (recommended).

    > **/!\\** Do only work with traefik < 2.0. The newer version of traefik uses routers and middleware to route queries which is incompatible in our stack definition here.

    I used traefik as a reverse proxy as the acme challenge with let's encrypt is built-in and really handy. This is not a mandatory step, you can expose container port and access your instance directly but I would not recommend it.

3. Copy the required files and fill your details:

    ```
    cp configs.yaml.sample configs.yaml
    cp db.env.sample db.env
    ```
    configs.yaml: allow to custom your instance
    ```
    ---
    domain: "localhost.localdomain"
    registry: <YOUR_REGISTRY_URL_HERE>
    fpm:
      version: 0.1
    proxy:
      version: 0.1
    ```
    The version variable here is just to track your modification made on your fpm and proxy instance. For example, I'm using a private registry such that each change is track and I can rollback to a previous change if anything go wrong. I can that way rebuild the exact same stack every time. Even if Nextcloud decide to update the image I'm using as a base, this will not impact me/you as the image is already built and store in my/your registry.

    db.env: database name and credentials. Make sure to use a strong password.

4. Deploy the nextcloud stack.

    ```
    make deploy

The stack should now be deployed, don't hesitate to open a PR and I'll gladly support you on this stack.

If you want to remove the containers (i.e restart), issue ```make remove```
