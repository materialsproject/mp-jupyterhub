Jupyterhub Setup For Materials Project Docker Image
===================================================

This service will allow you to spin up a multiuser Jupyter notebook environment with the materialsproject Docker image.
It uses Github OAuth to authenticate users.


### Prerequisistes
- Python 3
- Docker
- Node.js
- A Github Account


Jupyterhub requires Python 3. Either install your OS specific [Python 3](https://www.python.org/downloads/) or download the [Anaconda distribution](https://www.continuum.io/downloads).

You can get node.js from https://nodejs.org/en/


First get the Docker image: 
```
docker pull materialsproject/jupyterhub-singleuser
```

### Install and Setup

1. Install the requirements
   ```
   npm install -g configurable-http-proxy
   pip3 install -r requirements.txt
   ```
   This will install jupyter, jupyterhub, dockerspawner and oauthenticator


2. Setup your OAuth tokens in Github via https://github.com/settings/developers
   - Select "Register a New Application"
   - Make sure you fill out the callback URL as https://<yourdomain.org>/hub/oauth_callback (use http is you don't have SSL enabled)
   - Once registered note the Client ID and Client Secret

3. Edit `run/env.sh` and enter `GITHUB_CLIENT_SECRET`, `GITHUB_CLIENT_ID` and `OAUTH_CALLBACK_URL` from above

4. Edit the Jupyterhub config `run/jupyterhub_config.py` as needed. You may need to adjust values for if you wish to run with SSL:
   ```python

   c.JupyterHub.port = 443

   c.JupyterHub.ssl_cert = '/path/to/cert'
   c.JupyterHub.ssl_key = '/path/to/key'
   ```
   You may also adjust values for the shared directory from `/tmp` to your shared volume:
   ```python
   c.DockerSpawner.volumes = {
       '/tmp': '/wkshp_shared'
   }
   ```

5. Add initial users to `run/userlist`

### Run

If you have set appropriate SSL certs above you can leave off the `--no-ssl` below:
```
cd run
./run.sh --no-ssl
```










