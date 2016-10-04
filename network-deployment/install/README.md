# Building an IBM WebSphere Application Server Network Deployment traditional image from binaries

An IBM WebSphere Application Server Network Deployment traditional image can be built by installing Installation Manager and finding the correct repos.

An IBM WebSphere Application Server Network Deployment traditional install image is created in two steps by using the following two Dockerfiles to reduce the final image size:

1. [Dockerfile.prereq](Dockerfile.prereq)
2. [Dockerfile.install](Dockerfile.install)

Dockerfile.prereq takes the following actions:
 
1. Installs IBM Installation Manager
2. Installs IBM WebSphere Application Server 
3. Updates IBM WebSphere Application Server with the Fixpack
4. When the container is started a .tar file for the IBM WebSphere Application Server Network Deployment traditional  installation is created

Dockerfile.prereq takes the values for the following variables at build time: 
* user (optional, default is 'root') - user used for the installation
* group (optional, default is 'root') - group the user belongs to
* URL (not used) - URL from where the binaries are downloaded

Dockerfile.install takes the following action:

1. Extracts the .tar file created by Dockerfile.prereq

## Building the IBM WebSphere Application Server Network Deployment traditional image

1. Place the downloaded IBM Installation Manager and IBM WebSphere Application Server traditional binaries on the FTP or HTTP server
2. Clone this repository
3. Move to the directory `network-deployment/install`
4. Build the prereq image by using:

    ```bash
    docker build --build-arg user=<user> --build-arg group=<group> --build-arg URL=<URL> -t <prereq-image-name> -f Dockerfile.prereq .
    ```

5. Run a container by using the prereq image to create the .tar file in the current folder by using:

    ```bash
    docker run --rm -v $(pwd):/tmp -v /opt:/opt <prereq-image-name>
    ```

6. Build the install image by using:

    ```bash
    docker build --build-arg user=<user> --build-arg group=<group> -t nd -f Dockerfile.install .
    ```

