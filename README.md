
# 1 - Dependencies management {#1---dependencies-management}

***git branch name:*** dependencies

## Theory \[2\]

As usual, we will start with a few theoretical questions:

-   \[0.5\] What is Docker, and how it differs from dependencies
    management systems? From virtual machines?

Docker is a tool designed to make it easier to create, deploy, and run
applications by using containers. Containers allow a developer to
package up an application with all of the parts it needs, such as
libraries and other dependencies, and ship it all out as one package.

Docker differs from dependency management systems in that it is focused
on the deployment and execution of applications, rather than just
managing the dependencies of an application. Dependency management
systems, such as pip for Python or npm for JavaScript, are used to
manage the libraries and packages that an application depends on, but
they do not address the issue of how to run the application in different
environments.

Docker also differs from virtual machines in that it allows applications
to be packaged and run in a more lightweight and portable manner.
Virtual machines require a full operating system to be installed and
run, which can be resource-intensive, while containers share the host
operating system and only include the components necessary to run the
application. This makes containers more efficient and easier to manage
than virtual machines.

------------------------------------------------------------------------

-   \[0.5\] What are the advantages and disadvantages of using
    containers over other approaches?

There are several advantages to using containers over other approaches:

Portability: Containers allow applications to be easily moved between
different environments, such as development, testing, and production.
This can help streamline the software development process and make it
easier to deploy applications.

Isolation: Containers provide a level of isolation for applications,
allowing them to run in their own isolated environment. This can help
prevent conflicts between different applications and their dependencies.

Resource efficiency: Because containers share the host operating system,
they are more lightweight and resource-efficient than virtual machines,
which require a full operating system to be installed.

Consistency: Containers can help ensure that an application runs
consistently across different environments, which can be particularly
useful when working with microservices architectures.

There are also some potential disadvantages to using containers:

Complexity: Containerization can add complexity to the software
development process, particularly if the application consists of many
microservices that need to be containerized and orchestrated.

Security: Containers can potentially expose security vulnerabilities if
they are not properly configured or managed.

Limited control: Because containers share the host operating system,
they may have limited control over certain system resources, such as the
kernel or system libraries.

Overall, the decision to use containers should be based on the specific
needs and requirements of the application and the organization.
Containers can be a useful tool for many types of applications, but they
may not be the best fit in all cases.

------------------------------------------------------------------------

-   \[0.5\] Explain how Docker works: what are Dockerfiles, how are
    containers created, and how are they run and destroyed?

Docker works by using Dockerfiles to build images, which are then used
to create and run containers.

A Dockerfile is a text file that contains the instructions for building
a Docker image. It specifies the base image to use, the commands to run
to set up the application, and the necessary dependencies and
environment variables.

To create a Docker image, you can use the docker build command and
specify the path to the Dockerfile. The Docker engine will then execute
the instructions in the Dockerfile, creating an image that includes the
application and its dependencies.

Once an image has been created, you can use the docker run command to
create and start a new container based on that image. The container will
contain a copy of the application and its dependencies, and it can be
run on any machine that has Docker installed.

Containers can be stopped and restarted using the docker stop and docker
start commands, and they can be destroyed using the docker rm command.
When a container is destroyed, all of the data and resources associated
with it are also deleted.

------------------------------------------------------------------------

-   \[0.25\] Name and describe at least one Docker competitor (i.e., a
    tool based on the same containerization technology).

One Docker competitor is Kubernetes, which is an open-source container
orchestration system developed by Google.

Kubernetes is designed to automate the deployment, scaling, and
management of containerized applications. It allows you to define the
desired state of your application and its components, and it will
automatically ensure that the application is running as desired.
Kubernetes also provides features for load balancing, self-healing, and
rolling updates, which can make it easier to manage and maintain
containerized applications.

Kubernetes is widely used in production environments and is supported by
many cloud providers, making it a popular choice for container
orchestration.

------------------------------------------------------------------------

-   \[0.25\] What is conda? How it differs from apt, yarn, and others?

Conda is a package and environment management system for Python and
other languages, developed by Anaconda, Inc. It is designed to help
manage the dependencies and environments of a project and make it easier
to install and update packages.

Conda differs from other package management systems in a few ways:

Cross-language support: Conda supports packages for Python, R, and other
languages, in addition to supporting packages from multiple package
repositories.

Environment management: Conda allows you to create separate environments
for different projects, which can help prevent conflicts between package
dependencies.

Platform-agnostic: Conda is supported on Windows, macOS, and Linux, so
it can be used on a wide range of platforms.

Conda differs from package management systems like apt (for Debian-based
systems) and yum (for Red Hat-based systems) in that it is focused on
managing packages for Python and other languages, rather than
system-level packages. It also differs from package managers like yarn
(for JavaScript) and pip (for Python), which are primarily focused on a
single language.

Overall, Conda is a useful tool for managing dependencies and
environments for projects that use Python or other languages, and it can
be a helpful alternative to other package management systems.

## Problem \[6.5\] {#problem-65}

The problem itself is relatively simple.

Imagine that you developed an excellent RNA-seq analysis pipeline and
want to share it with the world. Based on your experience, you are
confident that the popularity of the pipeline will be proportional to
its ease of use. So, you decided to help your future users and to pack
all dependencies in a Conda environment and a Docker container.

Here is the list of tools and their versions that are used in your work:

-   [fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/),
    v0.11.9
-   [STAR](https://github.com/alexdobin/STAR), v2.7.10b
-   [reat](https://github.com/alnfedorov/reat), commit
    ee19bc928badd227975410a6b8b715c0e03bd4ab
-   [samtools](https://github.com/samtools/samtools), v1.16.1
-   [picard](https://github.com/broadinstitute/picard), v2.27.5
-   [salmon](https://github.com/COMBINE-lab/salmon), commit tag 1.9.0
-   [bedtools](https://github.com/arq5x/bedtools2), v2.30.0
-   [multiqc](https://github.com/ewels/MultiQC), v1.13

**Anaconda**:

-   \[1\] Install conda, create a new virtual environment, and install
    all necessary packages.
-   \[0.75\] You won\'t be able to install some tools - that\'s fine.
    List their names, and explain what should be done to make them
    conda-friendly
    ([conda-forge](https://conda-forge.org/docs/maintainer/adding_pkgs.html)
    channel,
    [bioconda](https://bioconda.github.io/contributor/workflow.html)
    channel).
-   \[0.25\]
    [Export](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#exporting-the-environment-yml-file)
    the environment
    ([example](https://github.com/nf-core/clipseq/blob/master/environment.yml))
    to the file and verify that it can be
    [rebuilt](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file)
    from the file without problems.

**Docker**:

-   \[3\] Create a Dockerfile for a container with **all** required
    dependencies. Don\'t forget about comments; test that all tools are
    accessible and work inside the container. Hints:
-   If needed, grant rights to execute downloaded/compiled binaries
    using chmod (`chmod a+x BINARY_NAME`)
-   Move all executables to \$PATH folders (e.g.`/usr/local/bin`) to
    make them accessible without specifying the full path.
-   Typical command to run a container interactively (`-it`) and delete
    on exit(`--rm`): `docker run --rm -it name:tag`
-   \[1\] Use [hadolint](https://hadolint.github.io/hadolint/) and
    remove as many reported warnings as possible.
-   \[0.5\] Add relevant
    [labels](https://docs.docker.com/engine/reference/builder/#label),
    e.g. maintainer, version, etc.
    ([hint](https://medium.com/@chamilad/lets-make-your-docker-image-better-than-90-of-existing-ones-8b1e5de950d))

### Anaconda

**Install anaconda and create an env**

    bash ~/Anaconda3-2022.10-Linux-x86_64.sh
    conda create --name homework -y
    conda activate homework

**Adding channels**

    conda config --add channels bioconda
    conda config --add channels conda-forge

**Installing packages**

    conda install -y fastqc=0.11.9
    conda install -y star=2.7.10b
    conda install -y salmon=1.9.0
    conda install -y samtools=1.16.1
    conda install -y bedtools=2.30.0
    conda install -y multiqc=1.13

Cant find Picard

**Export env**

    conda env export > environment.yml
    conda deactivate

\#\#\#\#Enviroment.yml


    name: homework
    channels:
      - conda-forge
      - bioconda
    dependencies:
      - fastqc=0.11.9 
      - star=2.7.10b
      - salmon=1.9.0
      - samtools=1.16.1
      - bedtools=2.30.0
      - multiqc=1.13

    prefix: /home/alkirdeev/anaconda3/envs/homework

###Docker

### Install Docker

    sudo apt update
    sudo apt upgrade
    sudo apt install docker.io
    sudo snap install docker

    touch Dockerfile
    nano Dockerfile


###Dockerfile

    FROM ubuntu:latest
    MAINTAINER "alkirdeev"

    #update links and install apt-utils
    RUN apt-get update
    RUN apt -y install apt-utils && \
        apt -y install apt-transport-https && \
        apt -y install openjdk-11-jre-headless && \
        apt -y install unzip && \
        apt -y install python3-pip

    #create /.bashrc for aliases
    RUN touch /.bashrc

    #run samtools v1.16.1
    RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip && \
        unzip samtools-1.16.1.zip && \
        rm samtools-1.16.1.zip && \
        mv samtools-1.16.1/misc samtools && \
        rm -r samtools-1.16.1 && \
        echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

    #run salmon v.1.9.0
    RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz && \
        tar -zxvf salmon-1.9.0_linux_x86_64.tar.gz && \
        rm salmon-1.9.0_linux_x86_64.tar.gz && \
        chmod a+x salmon-1.9.0_linux_x86_64/bin/salmon && \
        mv salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon && \
        rm -r salmon-1.9.0_linux_x86_64 

    #run bedtools v.2.30.0
    RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary && \
        chmod a+x /bin/bedtools.static.binary && \
        echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

    #run fastqc v0.11.9
    RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip && \
        unzip fastqc_v0.11.9.zip && \
        rm fastqc_v0.11.9.zip && \
        chmod a+x FastQC/fastqc && \
        echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

    #run STAR v.2.7.10b
    RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip && \
        unzip STAR_2.7.10b.zip && \
        rm STAR_2.7.10b.zip && \
        chmod a+x STAR_2.7.10b/Linux_x86_64_static/STAR && \
        mv STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR && \
        rm -r STAR_2.7.10b

    #run multic v1.13
    RUN pip install multiqc==1.13

**Creating a docker image**.

     sudo docker build -t ubuntu:latest
     sudo docker run --rm -it ubuntu:latest

**Changing the Dockerfile**

1.  MAINTAINER \--\> LABEL maintainer
2.  Remove the apt-get lists after installing anything
3.  apt \--\> apt-get install package=\"package_vesrion\"
4.  latest \--\> 20.04
5.  Reduce the number of steps

###Linter Labeled Optimazed Dockerfile

    FROM ubuntu:20.04
    LABEL maintainer="alkirdeev"

    LABEL org.label-schema.name="ht1"
    LABEL org.label-schema.description="HW1"

    # apt-transport-https, python-pip, and unzip
    RUN apt-get update && \
      apt-get install -y install apt-utils=2.4.8 && \
      apt-get install -y apt-transport-https=2.0.2ubuntu0.2 && \
      apt-get -y install openjdk-11-jre-headless=11.0.17+8-1ubuntu2~20.04 && \
      apt-get -y install python3-pip=20.0.2-5ubuntu1.5 && \
      apt-get -y install unzip=6.0-25ubuntu1.1 && \ 
      apt-get clean && \ 
      rm -rf /var/lib/apt/lists/*

    #create /.bashrc for aliases
    RUN touch /.bashrc

    #run samtools v1.16.1
    RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip && \
        unzip samtools-1.16.1.zip && \
        rm samtools-1.16.1.zip && \
        mv samtools-1.16.1/misc samtools && \
        rm -r samtools-1.16.1 && \
        echo 'alias samtools="/samtools/samtools.pl"' >> /.bashrc

    #run salmon v.1.9.0
    RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz && \
        tar -zxvf salmon-1.9.0_linux_x86_64.tar.gz && \
        rm salmon-1.9.0_linux_x86_64.tar.gz && \
        chmod a+x salmon-1.9.0_linux_x86_64/bin/salmon && \
        mv salmon-1.9.0_linux_x86_64/bin/salmon /bin/salmon && \
        rm -r salmon-1.9.0_linux_x86_64 

    #run bedtools v.2.30.0
    RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /bin/bedtools.static.binary && \
        chmod a+x /bin/bedtools.static.binary && \
        echo 'alias bedtools="/bin/bedtools.static.binary"' >> /.bashrc

    #run fastqc v0.11.9
    RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip && \
        unzip fastqc_v0.11.9.zip && \
        rm fastqc_v0.11.9.zip && \
        chmod a+x FastQC/fastqc && \
        echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc

    #run STAR v.2.7.10b
    RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip && \
        unzip STAR_2.7.10b.zip && \
        rm STAR_2.7.10b.zip && \
        chmod a+x STAR_2.7.10b/Linux_x86_64_static/STAR && \
        mv STAR_2.7.10b/Linux_x86_64_static/STAR /bin/STAR && \
        rm -r STAR_2.7.10b

    #run multic v1.13
    RUN pip install multiqc==1.13

## Extra points \[1.5\] {#extra-points-15}

You will be awarded extra points for the following:

-   \[0.5\] Using [multi-stage
    builds](https://docs.docker.com/build/building/multi-stage/) in
    Docker. E.g. to build reat and copy only the executable to the final
    image.

-   \[0.75\] Minimizing the size of the final Docker image. That is,
    removing all intermediates, unnecessary binaries/caches, etc. Don\'t
    forget to compare & report the final size before and after all the
    optimizations.

-   \[0.25\] Create an extra Dockerfile that starts from [a conda base
    image](https://hub.docker.com/r/continuumio/anaconda3) and builds
    everything from your conda environment file.

Hint: `conda env create --quiet -f environment.yml && conda clean -a`
([example](https://github.com/nf-core/clipseq/blob/master/Dockerfile))

Docker image:

-   via raw Dockerfile - **2.01 Gb**
-   via optimized Dockerfile - **1.32 Gb**

###Dockerfile for conda

    FROM continuumio/anaconda3
    LABEL maintainer="alkirdeev"
    #labels   
    LABEL org.label-schema.name="hw1"
    LABEL org.label-schema.description="HW1"

    #create the env    
    COPY environment.yml . 
    RUN apt-get update && apt-get -y install apt-utils=2.4.8 apt-transport-https=2.0.2ubuntu0.2 && \
      conda env create --file environment.yml && conda clean -a  
