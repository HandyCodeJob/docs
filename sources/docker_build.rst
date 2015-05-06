:title: Custom Containers with Docker Build
:description: Running minions in a Docker container defined by a Dockerfile
:keywords: shippable, Docker, Container

.. _docker_build:

.. note::
  Docker Build Support is with dedicated hosts only!

DOCKER SUPPORT
==============
Shippable is the world's only CI/CD platform built natively on Docker. All builds are run on Docker containers and this gives us a unique ability to support advanced docker workflows. We're constantly adding to our custom Docker support, so check back often!

**Connect your Docker Hub account to Shippable**
------------------------------------------------
If you want to interact with Docker Hub in any part of your build workflow, you need to connect your Docker Hub account to Shippable. This is a requirement for pulling custom images from your Docker Hub repos or pushing images to Docker Hub.

You can set up Docker Hub integration on a per-Org basis.

1. Go to your Organization's page on Shippable. You can find your Organization name at the right side of your Dashboard and click on it to go to the Org page.
2. If the 'Docker Hub' icon at the top right of your Org page is set to ON, you have already connected your Docker Hub account to your Org. If it is set to OFF, click on it, enter your Docker Hub credentials, and click on Save.

We do not call the Docker Hub API until we need to do so during an actual build, so we have no way of knowing if your credentials are correct at this point. The 'Docker Hub' icon will turn green after you save your creds, but if you run into problems with login to Docker Hub during the build, you should check your creds again.

-------

**push to Docker Hub**
----------------------

Shippable allows you to push an image to the docker registry after a successful build. To do this, make sure your Docker Hub icon is set to ON on your Organization's page on Shippable.

The following configuration in your shippable.yml will push the image to Docker Hub after the build is successful.

.. code-block:: bash

    commit_container: username/sample_project

The username above should be the same as the Docker Hub credentials you entered in the previous step.

-------

**Dockerbuild**
---------------

.. note::
  Docker Build Support is only available with dedicated hosts. To set up a dedicated host, please follow instructions `here <http://docs.shippable.com/en/latest/config.html#dedicated-hosts>`_

You can run your build in a custom docker container by building a Docker image from a Dockerfile. Aside from providing a custom environment for your build, this image created can be pushed to your Docker Hub account, for later use in your deployment step.

There are 2 ways to set up Docker build with Shippable - pre CI or post CI. 

Pre CI workflow is:
* Build the image using Dockerfile at the root of your repo
* Pull code from GitHub/Bitbucket and test code in the container
* Push container to docker hub

Post CI workflow is:
* Pull image specified from Docker Hub (default is minv2)
* Pull code from GitHub/Bitbucket and test in container
* If CI passs, build container from Dockerfile at the root of the repo
* Push container to docker hub

To use these workflows, your app must be "dockerized". Details on this can be found in Docker's official documentation `Docker's official documentation <https://docs.dockerhub.com>`_. You can also look at our `Docker build sample app <https://github.com/cadbot/dockerized-nodejs>`_. 

**Pre CI Dockerbuild**

* Make sure your Shippable org is connected to your Docker Hub account
* Enable the repository on Shippable
* On the repo page, go to 'Settings'. Choose the following -

  * Build image : Custom Image
  * Custom image action : Build
  * Custom image name : (docker hub username)/(image name)
  * Source code path : (source code path for image you want to build)
  * Push to Docker Hub : Check
* Make sure the Dockerfile for the image you want to build is at the root of your repo
* Trigger a manual or webhook build
* After the build is complete, make sure your Docker Hub account has the image you just pushed. The image should be tagged with the build number on Shippable.

