INSTALLATION
=============
To run the pipeline you need to install following software.

Java 8
-------
Java is required to run execution engine [Cromwell](https://software.broadinstitute.org/wdl/documentation/execution).
To check which Java version you already have, run:
```bash
java -version
```
You are looking for 1.8 or higher. If the requirement is not fulfilled follow installation instructions for [mac](https://java.com/en/download/help/mac_install.xml) or
[linux](http://openjdk.java.net/install/) or use your favorite installation method.

Docker
--------
Pipeline code is packaged and distributed in Docker containers, and thus Docker installation is needed. 
Follow instructions for [mac](https://docs.docker.com/docker-for-mac/install/) or (linux)[https://docs.docker.com/install/linux/docker-ce/ubuntu/#upgrade-docker-after-using-the-convenience-script]

Google Cloud
--------------
If you are intending to run the pipeline on Google Cloud platform, the following setup is needed:

DNA Nexus
-----------
If you are intending to run the pipeline on DNA Nexus, the following setup is needed: