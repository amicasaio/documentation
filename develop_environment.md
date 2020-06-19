# How to set up your local development environment

## Windows (pending)

## Linux

Here is the documentation for setting up your local development environment on different linux distros.

### Ubuntu

1. Install node & npm
   `sudo apt install nodejs && sudo apt install npm`
2. Install docker. Here we use a script already made by the docker team to configure docker on ubuntu. If you don't like to run scripts on your system blindfolded, you may see the file here: [https://get.docker.com](https://get.docker.com)
   `curl -fsSL https://get.docker.com -o get-docker.sh`
3. Install Kubernetes.
   `sudo snap install kubectl --classic`
4. Install Skaffold. Made by Google (open source), Skaffold is a command line tool that facilitates continuous development for Kubernetes-native applications.
   [Official documentation for installing skaffold](https://skaffold.dev/docs/install/)
5. Install minikube.
   [Official documentation for installing minikube](https://minikube.sigs.k8s.io/docs/start/)

## MacOS (pending)
