#code used to install local docker registry

docker run -d -p 5000:5000 --restart=always --name registry --network=host registry:2

#now we change the docker daemon to respond to the registry 

sudo nano /etc/docker/daemon.json

#add this in the file 

{
  "insecure-registries": ["(server ip address):5000"]
}

#restart the docker daemon to take the changes 

sudo systemctl restart docker

#commands to move around images in the local registry 

sudo docker tag (your-image) (server ip address):5000/(your-image)

sudo docker push (server ip address):5000/(your-image)

sudo docker pull (server ip address):5000/(your-image)

#update docker after changes to image 

sudo docker stop (your_container_name)

sudo docker rm (your_container_name)

sudo docker pull (server ip address):5000/(your-image)

sudo docker run -d --name (your_container_name) (server ip address):5000/(your-image)

#code to remove all containers 

docker rm -f $(docker ps -aq)

#script for github actions name it docker-build.yml

name: Docker Build and Push

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v2

    - name: Build Docker Image
      run: |
        docker build -t (your_image_name) .
        docker tag (your_image_name) (server ip address):5000/(your_image_name)

    - name: Push to Local Docker Registry
      run: |
        echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin (server ip address):5000
        docker push (server ip address):5000/(your_image_name)

#script for ansible name it deploy.yml

---
- name: Deploy Docker Container
  hosts: your_docker_server
  become: yes

  tasks:
    - name: Stop and Remove Existing Container
      docker_container:
        name: (your_container_name)
        state: stopped
        force_kill: yes
      ignore_errors: yes

    - name: Pull Updated Docker Image from Local Registry
      docker_image:
        name: (server ip address):5000/(your_image_name)
        source: pull

    - name: Run Docker Container
      docker_container:
        name: (your_container_name)
        image: (server ip address):5000/(your_image_name)
        ports:
          - "(host_port:container_port)"
        detach: yes


