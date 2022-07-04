# Jenkins Cheat Sheet
A simple guide to help you set up and configure a Jenkins server.
## Deployment
1. Run the following command to start Jenkins:
```bash
docker-compose up
```
2. Open a browser on http://localhost:8080
3. Run the initial setup wizard. Choose `recommended plugins`

Note that the admin password is dumped on the log.
## Configuration
* Github webhook
  * Your repository -> `Settings` -> `Webhooks` -> add a webhook `https://<jenkins-host>/github-webhook/` with content type `application/json`
  * Github will push events to that webhook URL (push, PR etc.)
* Jenkins
  * To create a new project, click `New item` and choose `Multibranch Pipeline`
    * `Branch Sources` -> `Add Source` -> `Github` -> enter Repository HTTPS URL
    * `Build Configuration` -> Mode: `by Jenkinsfile`, Script Path: `Jenkinsfile`
  * To registal credentials, go to `Manage Jenkins` -> `Manage Credentials` -> `Jenkins` -> `Global Credentials` -> `Add Credentials` -> enter username, password, and credential ID. In our example, the credential ID is `docker-hub`, which contains the dockerhub username and password
  * If Jenkins runs in a container, remember to mount host docker socket so that Jenkins could build and publish docker image in pipeline steps. Below is a docker-compose example.
  * A Jenkins build in a Multibranch Pipeline project will reference the Jenkinsfile of the ==corresponding branch==
      * For example, you push some code to branch `feature-a`, then Jenkins will trigger a build according to the Jenkinsfile in the branch `feature-a`