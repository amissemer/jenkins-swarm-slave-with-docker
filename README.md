# Jenkins swarm slave with Docker

Based on [`aratto/jenkins-swarm-slave`](https://hub.docker.com/r/aratto/jenkins-swarm-slave/).

An extendable [Jenkins swarm](https://wiki.jenkins-ci.org/display/JENKINS/Swarm+Plugin)
slave base to work with a [Jenkins master](https://hub.docker.com/r/amissemer/git-sync-jenkins/) with the Swarm plugin enabled, and Docker CLI installed.

## Running

Just use Docker run with any argument the slave
[accepts](https://wiki.jenkins-ci.org/display/JENKINS/Swarm+Plugin#SwarmPlugin-AvailableOptions):

    docker run -v /var/run/docker.sock:/var/run/docker.sock amissemer/jenkins-swarm-slave-with-docker -master http://jenkins:8080 -username jenkins -password jenkins -executors 1

By specifying `-v /var/run/docker.sock:/var/run/docker.sock` the slave will be able to access its host Docker engine and thus control containers on that host.

Linking to the Jenkins master container there is no need to use `-master`

    docker run -d --name jenkins -p 8080:8080 csanchez/jenkins-swarm
    docker run -d -v /var/run/docker.sock:/var/run/docker.sock --link jenkins:jenkins amissemer/jenkins-swarm-slave-with-docker -username jenkins -password jenkins -executors 1

## Extending

When running be sure to pass relevant `-labels` options to manage what types of
build will be serviced.