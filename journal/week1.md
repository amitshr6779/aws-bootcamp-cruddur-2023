# Week 1 — App Containerization


## Run the dockerfile CMD as an external script

Added script and run it entrypoint
```
FROM python:3.10-slim-buster

# Inside Container
# make a new folder inside container
WORKDIR /backend-flask

# Outside Container -> Inside Container
# this contains the libraries want to install to run the app
COPY requirements.txt requirements.txt

# Inside Container
# Install the python libraries used for the app
RUN pip3 install -r requirements.txt

# Outside Container -> Inside Container
# . means everything in the current directory
# first period . - /backend-flask (outside container)
# second period . /backend-flask (inside container)
COPY . .

# Set Enviroment Variables (Env Vars)
# Inside Container and wil remain set when the container is running
ENV FLASK_ENV=development

EXPOSE ${PORT}

RUN chmod 777 start_app.sh

ENTRYPOINT ["sh", "./start_app.sh"]

```

Then build the image  and run the conatiner.

```
docker build -t backend .
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend
```

![Screenshot from 2023-02-26 20-25-35](https://user-images.githubusercontent.com/84858868/221418354-3541b820-5886-44aa-98d9-d2ae75f23183.png)




![Screenshot from 2023-02-26 20-30-58](https://user-images.githubusercontent.com/84858868/221418580-7cbb5fb8-52bc-4102-a238-c2900e29d1a8.png)


## Push and tag a image to DockerHub

Created image and tagged it and then pushed it to Docker hub.

![Screenshot from 2023-02-26 20-38-11](https://user-images.githubusercontent.com/84858868/221419090-4a1ac42b-eab7-4876-9ab2-5f412f1b5c79.png)


![Screenshot from 2023-02-26 20-37-08](https://user-images.githubusercontent.com/84858868/221419104-e95e5d01-5f89-405d-bf30-6299f4000dea.png)


<br>

## Use multi-stage building for a Dockerfile build

Learned about multi stage and Created muti stage Dockerfile for backend app

```
FROM python:3.10-slim-buster  AS BuildStage

WORKDIR /backend-flask

COPY requirements.txt requirements.txt

RUN pip3 install -r requirements.txt

COPY . .


FROM python:3.10-slim-buster  AS FinalStage

WORKDIR /backend-flask

COPY --from=BuildStage /backend-flask .

RUN pip3 install -r requirements.txt

ENV FLASK_ENV=development

EXPOSE ${PORT}

RUN chmod 777 start_app.sh

ENTRYPOINT ["sh", "./start_app.sh"]

```

<br>

## Implement a healthcheck in the V3 Docker compose file

Impleted heathchek in backed app and tested is healthy or not !


![Screenshot from 2023-02-26 21-54-31](https://user-images.githubusercontent.com/84858868/221423116-fde38cc6-59df-48ed-8460-5f84b1c42073.png)

![Screenshot from 2023-02-26 21-54-46](https://user-images.githubusercontent.com/84858868/221423130-0e5d6605-2270-4247-aef4-14eb06400f1c.png)




## install Docker on your localmachine

Installed docker and docker-compose in local  using following coomnd.

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
   ```
   ```
   sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
  sudo apt-get update
  sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
   
   Also, checkd the docker service running or not !

![Screenshot from 2023-02-26 22-15-12](https://user-images.githubusercontent.com/84858868/221424163-b9740610-3ea8-49e0-aae6-aee31c8ad4de.png)


## Launch an EC2 instance that has docker installed,

Launch an ec2 and  installed docker by passing  insatllation script through  user data of ec2.


![Screenshot from 2023-02-26 22-21-05](https://user-images.githubusercontent.com/84858868/221425581-31c81956-80a2-47b9-bf86-4dca2bc67642.png)


![Screenshot from 2023-02-26 22-39-00](https://user-images.githubusercontent.com/84858868/221425594-d9e3c7e7-0cd2-42fa-957c-c9d873ce1688.png)



## Best practices of Dockerfiles
* Proper tag you Image
* Dont give root access to conatiner , insted run container with separte user
* Use Multi Staging
* Do not install unnessary package 
* Use docker .dokcerignore
* Sort multi-line arguments























