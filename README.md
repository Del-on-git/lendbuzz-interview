# How this works:

There are two workflows for this project, both housed under `.github/workflows`. These workflows are triggered on a push or merge to either the `main` or `master` branches.

## `containerize-app.yml`
Logs into the artifactory at delizardo.jfrog.io with a docker client running on the latest ubuntu container available to GitHub Actions, then builds the app from `docker/app/Dockerfile` and pushes the image to the `interview-app-local` respository with two tags. One tag provides an archived version for rollbacks, the other tag is always `latest` and destined for deployment.

##  `deploy.yml`
Depends on the successful completion of `containerize-app` workflow. Logs into the host for for the app (an EC2 t2 micro instance hosted on AWS) over SSH, then logs that hosts docker client into the artifactory at delizardo.jfrog.io and pulls the `latest` image of the app. After downloading the app image, docker compose prunes the old (now untagged) app image and restarts the services for postgres and the new app image.

## Hit the endpoints
You can see the latest working version of the app deployed to the host by visiting (or cURLing) http://34.232.230.4/ and http://34.232.230.4/db_healthcheck

# Instructions to run the app locally
1. Install Node (Google Instructions)

2. Install Dependencies for the App
```sh
npm install
```

3. Run the app
```sh
npm start
```

(Optional)
```sh
DB_NAME={name_of_db} DB_HOST={host_url_of_db} DB_USER={username_of_db} DB_PASS={password_of_db} npm start
```
to run `/db_healthcheck` endpoint
