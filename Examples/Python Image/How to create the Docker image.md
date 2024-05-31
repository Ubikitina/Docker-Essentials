# Containerizing Python projects

This file provides instructions on how to containerize a Python project using Docker. 

I have developed this section using the instructions from the article "How To Use Docker To Containerize Your Python Project" (https://python.land/deployment/containerize-your-project#Dockerize_yournbspPython_project), specifically the section "Dockerize your Python project".

To create the Docker image, use the following command:
```bash
docker build -t python_test:v1 .
```

To create a container from the image, use the following command:
```bash
docker run -it python_test:v1
```