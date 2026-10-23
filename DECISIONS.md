# DECISIONS.md

## MongoDB version

`mongo:8` didn't work on the EC2 instance, so I switched to `mongo:7.0.43-jammy`. The database started working with this version.

## MongoDB connection

The backend connects to MongoDB using `mongodb://mongodb:27017`.

Here, `mongodb` is the service name in Docker Compose. Using `localhost` would point to the backend container itself, not the MongoDB container.

## Frontend API issue

The frontend was sending requests to `localhost:8000`. This worked locally, but caused problems when the app was opened through EC2 because the browser treated localhost as the user's own computer.

Changed the API URL to `/api` and configured Nginx to forward those requests to the backend.

## MongoDB port

MongoDB doesn't need to be exposed to the internet. The backend can reach it through the Docker network, so the database port wasn't published on the EC2 host in the production Compose file.

## Jenkins deployment to EC2

Tried deploying to EC2 through Jenkins over SSH, but ran into SSH key permission issues on Windows. Dropped this approach rather than spending more time troubleshooting it.

The EC2 deployment was tested manually using Docker Compose. Automated Jenkins deployment to EC2 was not completed.

## AWS costs

The AWS resources were deleted after testing to avoid further charges. The deployment is no longer live.

## Things to improve later

* Move Jenkins to Linux and use shell commands instead of Windows `bat` commands.
* Use versioned Docker image tags instead of `latest`.
* Set up automated deployment and a reliable rollback process.
