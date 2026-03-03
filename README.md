# Flask for Magic Containers

A Flask app served with Gunicorn and Bunny CDN header detection, ready to deploy on [Bunny Magic Containers](https://bunny.net/magic-containers/).

## What's included

- `app.py` - Flask application with routes and Bunny CDN header detection
- `requirements.txt` - Python dependencies (Flask, Gunicorn)
- `Dockerfile` - Container image using the official Python image with Gunicorn
- `docker-compose.yml` - Local development setup
- `bunny.json` - Magic Containers app config
- `.github/workflows/deploy.yml` - GitHub Actions workflow to build, push to GitHub Container Registry, and deploy to Magic Containers

## Run locally

```bash
docker compose up
```

Visit [http://localhost:8000](http://localhost:8000).

## Deploy to Magic Containers

### 1. Fork and push

Fork this repository and push to the `main` branch. The GitHub Actions workflow will automatically build the Docker image and push it to `ghcr.io/<your-username>/mc-template-flask` tagged with both `latest` and the commit SHA.

### 2. Make the package public

Go to your GitHub profile > **Packages** > select the `mc-template-flask` package > **Package settings** > change visibility to **Public**.

### 3. Create an app on Magic Containers

1. Log in to the [bunny.net dashboard](https://dash.bunny.net) and navigate to **Magic Containers**.
2. Click **Create App**.
3. Add the **app** container:
   - **Registry**: GitHub Container Registry (`ghcr.io`)
   - **Image**: `ghcr.io/<your-username>/mc-template-flask:latest`
   - Add an **Endpoint** on port `8000`
4. Confirm and deploy.

### 4. Test it

Once deployed, you'll get a `*.bunny.run` URL:

```bash
curl https://mc-xxx.bunny.run
```

The Bunny CDN headers (`cdn-requestcountrycode`, `cdn-requestid`) will be detected automatically when running behind Bunny CDN.

## Continuous deployment

The workflow automatically deploys to Magic Containers on every push to `main`. Configure the following in your repository settings:

- **Variable** `APP_ID` - your Magic Containers app ID
- **Secret** `BUNNYNET_API_KEY` - your bunny.net API key
