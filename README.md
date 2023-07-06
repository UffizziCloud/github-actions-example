# Uffizzi Quickstart for Kubernetes (~ 1 minute)

Go from pull request to Uffizzi Preview Environment on Kubernetes in less than one minute...

### 1. Fork the `quickstart_k8s` repo

⚠️ Be sure to <ins>**uncheck**</ins> the option **Copy the `main` branch only**.

This ensures that the `try-uffizzi` branch will be included in your fork.  

### 2. Enable GitHub Actions workflows for your fork

Select **Actions**, then select **I understand my workflows, go ahead and enable them**.

### 3. Open a pull request for the `try-uffizzi` branch against `main` in your fork

⚠️ Be sure that you're opening a PR on the branches of _your fork_ (i.e. `your-account/main` ← `your-account/try-uffizzi`).

If you try to open a PR for `UffizziCloud/main` ← `your-account/try-uffizzi`, the Actions workflow will not run in this example.

### 4. Open the completed action summary page

Under the Actions section of the forked repository, open up the latest completed action. Within the completed action, open up the summary section

### 5. Access the vote and result endpoints

Copy and paste the vote and result endpoints within the action summary

<img src="https://github.com/UffizziCloud/quickstart-k8s/blob/readme/images/quickstartss.png" width="800">

___
## What to expect

The PR will trigger a [GitHub Actions workflow](.github/workflows/uffizzi-preview.yaml) that uses the Uffizzi CLI and Kubernetes manifests to create a Uffizzi Preview Environment for the [microservices application](#architecture-of-this-example-app) defined by this repo. The Preview Environment URL will be posted as a comment in your PR issue when the workflow completes.

## How it works

### Configuration

Preview Environments are configured with [Kubernetes manifests](kustomization.yaml) that describe the application components and a [GitHub Actions workflow](.github/workflows/uffizzi-preview.yaml) that includes a series of jobs triggered by a `pull_request` event and subsequent `push` events:

1. [Build and push the voting-app images](https://github.com/UffizziCloud/quickstart-k8s/blob/c6123e3510e69a9433398eeb59482d19b920fcee/.github/workflows/create-ucluster.yaml#L11C3-L36C26)
2. [Create the Uffizzi cluster using the uffizzi-cli](https://github.com/UffizziCloud/quickstart-k8s/blob/c6123e3510e69a9433398eeb59482d19b920fcee/.github/workflows/create-ucluster.yaml#L103C1-L112C36)
3. [Apply the Kubernetes manifests to deploy the application to the Uffizzi cluster](https://github.com/UffizziCloud/quickstart-k8s/blob/c6123e3510e69a9433398eeb59482d19b920fcee/.github/workflows/create-ucluster.yaml#L125C1-L125C73)
4. Delete the Preview Environment when the PR is merged/closed or after

<!-- ### Uffizzi Cloud

Running this workflow will create a [Uffizzi Cloud](https://uffizzi.com) account and project from your GitHub user and repo information, respectively. If you sign in to the [Uffizzi Dashboard](https://app.uffizzi.com/sign_in), you can view logs, password protect your Preview Environments, manage projects and team members, set role-based access controls, and configure single-sign-on (SSO).

Open-source projects preview for free on Uffizzi Cloud. All other accounts can subscribe to our Starter or Pro plans. See [our pricing](https://uffizzi.com/pricing) for details. If you're an open-source maintainer, you can request free access by sending an email to opensource@uffizzi.com. Alternatively, if you don't want to use Uffizzi Cloud, you can [install open-source Uffizzi](https://github.com/UffizziCloud/uffizzi_app/blob/develop/INSTALL.md) on your own Kubernetes cluster. -->

## Acceptable Use

We strive to keep Uffizzi Cloud free or inexpensive for individuals and small teams. Therefore, activities such as crypto mining, file sharing, bots, and similar uses that lead to increased cost and intermittent issues for other users are strictly prohibited per the [Acceptable Use Policy](https://uffizzi.zendesk.com/hc/en-us/articles/4410657390999-Acceptable-Use-Policy). Violators of this policy are subject to permanent ban.

## Architecture of this Example App

The application defined by this repo allows users to vote for dogs or cats and see the results. It consists of the following microservices:

<img src="https://user-images.githubusercontent.com/7218230/192601868-562b705f-bf39-4eb8-a554-2a0738bd8ecf.png" width="400">

* **voting-app** - A frontend web app in [Python](/vote) that lets you vote between two options
* **redis** - A [Redis](https://hub.docker.com/_/redis/) queue that collects new votes
* **worker** - A [.NET Core](/worker/src/Worker) worker that consumes votes and stores them in...
* **db** - A [PostgreSQL](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* **result-app** - A [Node.js](/result) web app that shows the results of the voting in real time
