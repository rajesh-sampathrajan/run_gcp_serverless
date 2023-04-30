## Introduction

In this tutorial, I will show you how to host Backstage in GCP using Cloud Run, a serverless platform for deploying and scaling containerized applications. Backstage is an open-source platform for building developer portals. It is designed to help teams manage their software development lifecycle by providing a centralized place to manage services, tooling, and documentation.

Before we get started, you will need a GCP account, a project in GCP, and the GCP CLI tool, `gcloud`, installed on your local machine.

## Step 1: Set up a GCP project

To get started, I logged in to my GCP console and created a new project. I did this by clicking the "Select a Project" dropdown menu in the top navigation bar and selecting "New Project". I then followed the prompts to create the project.

Once I created the project, I navigated to the project dashboard and enabled the necessary APIs. Specifically, I needed to enable the following APIs:

- Cloud Run
- Secret Manager
- Container Registry

## Step 2: Configure the Backstage app

Next, I needed to configure the Backstage app for deployment to GCP. I did this by cloning the Backstage repository and making the necessary changes to the configuration files.

```bash
$ git clone https://github.com/backstage/backstage.git
$ cd backstage
```

I opened the `packages/app-config/app-config.yaml` file and set the following values:

```yaml
proxy:
  baseUrl: '<your-cloud-run-url>'
  ...
```

This set the base URL for the Backstage app to my Cloud Run URL.

Next, I opened the `Dockerfile` and updated the `BASE_URL` environment variable with my Cloud Run URL:

```Dockerfile
ARG NODE_ENV=production
ENV NODE_ENV $NODE_ENV
ENV PORT 8080
ENV BASE_URL '<your-cloud-run-url>'
```

I saved the changes to both files and committed them to my local Git repository.

## Step 3: Build and push the Docker image

Next, I needed to build and push the Docker image to GCP Container Registry.

To build the Docker image, I ran the following command:

```bash
$ docker build -t gcr.io/<your-project-id>/backstage .
```

This built a Docker image with the tag `gcr.io/<your-project-id>/backstage`.

Next, I pushed the Docker image to GCP Container Registry by running the following command:

```bash
$ docker push gcr.io/<your-project-id>/backstage
```

This pushed the Docker image to the `gcr.io` container registry in my GCP project.

## Step 4: Deploy the Backstage app to Cloud Run

Finally, I was ready to deploy the Backstage app to Cloud Run.

To deploy the app, I ran the following command:

```bash
$ gcloud run deploy backstage \
  --image gcr.io/<your-project-id>/backstage \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated
```

This deployed the Backstage app to Cloud Run in the `us-central1` region.

Once the app was deployed, I could access it by visiting the URL provided by Cloud Run. If I needed to make changes to the app configuration, I could update the configuration files and redeploy the app using the same process.

## Conclusion

In this tutorial, I showed you how to host Backstage in GCP using Cloud Run. You can set up a GCP project, configur the Backstage app, build and push a Docker image to GCP Container Registry, and deploy the app to Cloud Run.
