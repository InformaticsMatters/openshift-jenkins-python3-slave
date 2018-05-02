# An OpenShift Jenkins Slave Agent Image for "Python 3.6"
This is a Jenkins slave image built on the OpenShift Jenkins Base
slave that adds [Python] to the image. It should be
suitable for OpenShift Origin v3.6/v3.7 deployments. A matching
**ImageStream** template is also included for the `latest` image.

>   For background material you can refer to the OpenShift documentation for
    their [Jenkins] service and general information on [builds] and image
    streams.

>   The Python installed is defined by the OpenShift **base** image and
    is (at the time of writing) `3.6.5`.

## Building the image
I use, and you might need...

-   Docker Engine `18.03.1-ce`
-   Docker Compose `1.21.0`
    
Build and tag the image...

    $ docker-compose build

## Deploying the image
Here, we'll deploy to the Docker hub. It's free and simple. We just need to
push it using `docker-compose`.

    $ docker login -u informaticsmatters
    [...]
    $ docker-compose push

## Using the image as an OpenShift Jenkins slave
To use the image as an OpenShift Jenkins slave you can use the accompanying
`slave.yaml` template file to create a suitable **ImageStream** using the `oc`
command-line. Assuming you're logged into the OpenShift server and the project
into which you've installed Jenkins you can run the following command to add
a suitable image stream: -

    $ oc process -f slave.yaml | oc create -f -

This should result in a `python-slave` **ImageStream** that can be used as a
source for a Jenkins slave. To employ the image as a slave you refer to it is
as the `agent` in your Jenkins _pipeline_ where you'd normally refer to an
agent:

    agent {
      label 'python3-slave'
    }

>   You may need to re-deploy Jenkins (_bounce its Pod_) because it only looks
    for slave-based image streams once, during initialisation. So, if you add a
    slave stream after jenkins was started it'll need to be restarted. When the
    image stream has been recognised you should find it on the
    `Jenkins -> Manage Jenkins -> Configure System` page as a new image
    (with your chosen Name) in the `Kubernetes` section.

---

[builds]: https://docs.openshift.com/container-platform/3.6/architecture/core_concepts/builds_and_image_streams.html
[jenkins]: https://docs.openshift.com/container-platform/3.6/using_images/other_images/jenkins.html
[python]: https://www.python.org
