# Lab 04 - Continuous Delivery using Docker

> **Dificulty**: Moderate

> **Time**: 34 minutes

> **Tasks**
- [Prerequisite](#Prerequisite)
- [Task 1: Forking the repository](#task-1-forking-the-repository)
- [Task 2: Docker Hub create an automated build](#task-2-docker-hub-create-an-automated-build)
- [Task 3: Start the application](#task-3-start-the-application)
- [Task 4: Start a Jenkins container](#task-4-start-a-jenkins-container)
- [Task 5: Jenkins basic installation and configuration](#task-5-jenkins-basic-installation-and-configuration)
- [Task 6: Create a Jenkins deploy job](#task-6-create-a-jenkins-deploy-job)
- [Task 7: Decrease Jenkins security](#task-7-decrease-jenkins-security)
- [Task 8: Create Docker Hub webhook](#task-8-create-docker-hub-webhook)
- [Task 9: Continuous Delivery in practice](#task-9-continuous-delivery-in-practice)

## Prerequisite

Before procedding with this lab, please create an account on [GitHub](https://github.com/join)

Also, before proceeding with this lab, please familiarize yourself first with the topics below:

* [Continuous Delivery](https://en.wikipedia.org/wiki/Continuous_delivery)

## Task 1: Forking the repository 

Using a browser surf to https://github.com/gluobe/container-info and click the `Fork` button on the top right of the page.  This should take less than a minute and aftwerwards you will have a fork of the gluobe/container-info repository under you own GitHub account.

## Task 2: Docker Hub create an automated build

* Surf to [https://hub.docker.com/](https://hub.docker.com/)
* Login if not already logged-in
* Click the `Create` button on the top right
* Click the `Create Automated Build` option
* Link your GitHub account if not already linked
* Create an Auto-build for GitHub
* Select the correct user/organization and repository
* Add a description and click the `Create` button

## Task 3: Start the application

The automated build should be triggered automatically, because we are using a free account you might have to wait a couple of minutes before the build starts.  Once the build complete you can start the conatiner with the following command:

```
docker run -d -p 80:80 --name container-info <DOCKERHUB_USERNAME>/container-info
```

NOTE:
- we are adding the `--name` tag so we can more easily stop and start the container
- it is important that you are starting your own container, so make sure you replace <DOCKERHUB_USERNAME> with your Docker Hub username

## Task 4: Start a Jenkins container 

Start a Jenkins container using the following command:

```
docker run -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker -v /usr/lib/x86_64-linux-gnu/libapparmor.so.1.1.0:/lib/x86_64-linux-gnu/libapparmor.so.1 --name jenkins trescst/jenkins
```

NOTE: do not worry to much about the `-v` options, these are required to run Docker inside of Docker (this is out-of-scope for an introduction course)

## Task 5: Jenkins basic installation and configuration 

Surf to http://nodeXY.docker.gluo.io:8080, you will see that you need to enter a secret first.  To get the secret you will need to run a command inside the container, this can easily be done using the following command:

```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Click the left button to install the most common plugins.  When the installation of the additional plugins is finished create an admin user (use `admin` for both username and password).

## Task 6: Create a Jenkins deploy job 

* Click the `create new job` links
* Enter a name (for example: container-info-deploy) and select `Freestyle project` and click the `OK` button
* Scroll down to the `Build` section
* Click the `Add build step` and select the `execute shell` option
* In this field copy/paste:

```
# stop and remove the container-info container
docker stop container-info
docker rm container-info

# pull the latest container-info image
docker pull trescst/container-info

# start a new container-info container (using the latest image)
docker run -d -p 80:80 --name container-info <DOCKERHUB_USERNAME>/container-info
```
* Scroll back up to the `Build Triggers` section, and select `Trigger builds remotely (e.g., from scripts)`
* Enter a random TOKEN_NAME, for example "ChooseYourOwnToken"
* Click the `Apply` button and then the `Save` button

## Task 7: Decrease Jenkins security

For demo purposes we will allow anonymous access, this is required for our webhook to work.  This is, of course, not a recommended setup for production environments.

* Click the Jenkins logo on the top left
* Click the `Manage Jenkins` link
* Click the `Configure Global Security`
* Under `Access Control` and `Authorization` select the `Anyone can do anything` option
* Click the `Apply` button and then the `Save` button

## Task 8: Create Docker Hub webhook 

* Surf to [https://hub.docker.com/](https://hub.docker.com/)
* Login if not already logged-in
* Search for your image an click `Details` 
* Click the `Webhooks` tab
* Add a webhook similar to: `http://nodeXY.docker.gluo.io:8080/job/container-info-deploy//build?token=ChooseYourOwnToken`

## Task 9: Continuous Delivery in practice

* Surf to http://nodeXY.docker.gluo.io and notice the color of your box
* Surf to [GitHub](https://github.com) and select your fork of the container-info repository
* Click on the `index.php` file
* Click the edit icon (top right)
* Search for "CHANGE COLOR BELOW" and change the color
* Add a commit message on the bottom, for example "changed color to pink"
* Click the `Commit Changes` button
* Go to you image on Docker Hub, click `Details` and click the `Build Details` tab (a new build should have been triggered, it will be in either queued or building state)
* As soon as the build is finished you can have a look in Jenkins where the job te redeploy the application will have been triggered

## Conclusion

Congratulations, You have successfully completed this lab! You learned how to use Docker, Jenkins and GitHub to setup an automated continuous delivery pipeline.


## Cleanup

Please clean up your environment by running the following commands:

```
docker stop $(docker ps -qa)
docker rm -f $(docker ps -qa)
docker rmi -f $(docker images -qa)
```
