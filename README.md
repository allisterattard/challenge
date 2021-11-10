# challenge
## Task 1

For the first task I had created 2 Dockerfiles, 1 in api directory and another in sys-stats directory. 
For api docker image I had user python 3.9.7 slim version so to keep the image as light as possible. For sys-stats UI I had create the first image on latest node version and set this part of the dockerfile with webapp label. During this part react webapp is installing node modules need via npm and copy src and public dir from host to be able to build the react webapp. After this part is completed I set to copy the build done in the previous part and copy it to the nginx html dir, Since I had decided to use nginx image for serve the react webapp as a web server for it. I chose nginx alpine to keep the final image as light as possible. 

To build the images you can go in task1/ dir and run

```
docker-compose build
```

TO build image and start apps at the same time to go task1/ dir and run

```
docker-compose up -d --build
```

## Task 2

For this task I had used Ionos as a cloud provider, had set up a Centos machine and installed docker on it. 
I decided to use nginx as a reverse proxy so I had uploaded nginx.conf on the machine so I can mount it to the docker container as a volume. Also, I had set all the needed var in /etc/ansible/hosts.
For this task I had to rebuild the images done in task1 since the React webapp was set to call the python api user localhost:5000, I had set it to call my cloud machine IP on port 80.
In the ansible playbook I used the docker container module, I had set the container name for each one, together with the image they will be using. For all 3 I had set restart policy always so if for any reason one container crashes it will try to restart it for that downtime will be as minimum as possible.

To run the ansible playbook run

```
ansible-playbook -i /etc/ansible/hosts playbook-docker-mychal.yml
```

You can access it via the following URL

```
http://87.106.113.71/
```

## Task 3

For this task I had used minikube locally. I had set enable ingress on my minikube setup. All deployment, service and ingress config are set in one file instead on multiple files. For both webapp and python api I had set replica as1 and image pull policy to never to it won't try to get image from docker hub since images are just present on my local machine. Also I had set the service to use NodePort I was will be able to test apps individually if needed. For the ingress config I had set the host to be chal.example.com and I had updated my local host file so the dns will point to my local machine IP. Also, in the ingress section I had set u paths were / is pointing to the webapp pod and /stats points to the python api pod.

To enable minikube ingress

```
minikube addons enable ingress
```

Load images from docker images to minikube

```
minikube image load aattard_api

minikube image load aattard_web
```

Deploy apps on minikube

```
minikube kubectl -- apply -f deployment_service_ingress.yaml
```

## Improvements

For improvements I would have Set a proper CI/CD pipeline to test and build both docker images and pushed them to any docker registry (docker hub or example nexus/gitlab for self-hosted). Also Ideally React app will not have python endpoint hardcoded, but it should get the dns form the browser so no need to change configs between envs.
Also once app is deployed SSL and a proper DNS will be set for it, and if app is intended for monitoring use allowed list can be added on nginx.
