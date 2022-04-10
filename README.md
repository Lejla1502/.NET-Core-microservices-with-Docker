## Sample for the Build your first microservice in .NET learn module

Microservice applications are composed of small, independently versioned, and scalable customer-focused services that communicate with each other over standard protocols with well-defined interfaces. Each microservice typically encapsulates simple business logic, which you can scale out or in, test, deploy, and manage independently.  Smaller teams develop a microservice based on a customer scenario and use any technologies that they want to use. This module will teach you how to build your first microservice with .NET. <br/>

In order to run the code in this repo as a microservice, please complete the [Build your first microservice in .NET](https://docs.microsoft.com/learn/modules/dotnet-microservices).

### Steps to run

1. You will need to add the **Dockerfile** to the web API in the backend folder.
1. And add the **docker-compose.yml** file to the root folder. <br/>

Along with the necessary Docker commands for building an image and running Docker compose as well. <br/>

Check out the Learn module to find out all about [Building your first microservice in .NET](https://docs.microsoft.com/learn/modules/dotnet-microservices).

## Docker file

The following will be performed sequentially when Docker file is invoked: <br/>

Pull the mcr.microsoft.com/dotnet/sdk:6.0 image and name the image build <br/>
Set the working directory within the image to /src <br/>
Copy the file named backend.csproj found locally to the /src directory that was just created <br/>
Calls dotnet restore on the project <br/>
Copy everything in the local working directory to the image <br/>
Calls dotnet publish on the project <br/>
Pull the mcr.microsoft.com/dotnet/aspnet:6.0 image <br/>
Set the working directory within the image to /app <br/>
Exposes port 80 and 443 <br/>
Copy everything from the /app directory of the build image created above into the app directory of this image <br/>
Sets the entrypoint of this image to dotnet and passes backend.dll as an argument <br/>

We've copied .csproj file separately and then ran dotnet restore, while dotnet publish command would have done it all. we've separated this to optimize
our image building process. Docker creates separate layer for each instruction in the file. The layers are stacked and each one is a delta of the 
changes from the previous layer. Docker also caches these layers so it doesn't hav to rebuild the layer when it is unchanged. That's why we've copied 
.csproj file and ran dotnet restore first. So all the packages are cached within that layer and they don't have to be relearn when we rebuild image. <br/>

To run the web API service, run the following command to start a new Docker container using the pizzabackend image and expose the service on port 5200: <br/>
docker run -it --rm -p 5200:80 --name pizzabackendcontainer pizzabackend <br/>
You can browse to http://localhost:5200/pizzainfo and see a JSON representation of Contoso Pizza's menu.

## Docker Compose

To run multiple Docker files locally, we use docker compose tool. This tool allows us to build and run all services with a single command, instead of doing
docker build and docker run for every container. By default, it also sets up a single network for the app. Each cotainer joins the default network and is eachable by other containers on the network.  <br/>

The code from Docker compose yaml does several things: <br/>
  -First, it creates the frontend website, naming it pizza frontend. The code tells Docker to build it, pointing to the Dockerfile found in the frontend folder. Then the code sets an environment variable for the website: backendUrl=http://backend. Finally, this code opens a port and declares it depends on the backend service. <br/>
  -The backend service gets created next. It's named pizzabackend. It's built from the same Dockerfile. The last command specifies which port to open.
  
## Kubernetes

In order for Kubernetes to create a container image, it needs a place to get it from. Docker Hub is a central place to upload Docker images. Many products, including Kubernetes, can create containers based on images in Docker Hub. That is why it is needed to push local docker image to Docker Hub. 
In order to deploy container image to Kubernetes, we create yaml files for backend and frontend where we define what we want Kubernetes to do. <br/>

Steps:
  1. docker login
  2. docker tag pizzafrontend [YOUR DOCKER USER NAME]/pizzafrontend <br/>
     docker tag pizzabackend [YOUR DOCKER USER NAME]/pizzabackend
  3. docker push [YOUR DOCKER USER NAME]/pizzafrontend <br/>
     docker push [YOUR DOCKER USER NAME]/pizzabackend
 <br/>
To download image from Docker Hub and create container: <br/>
      kubectl apply -f backend-deploy.yml <br/>
      kubectl apply -f frontend-deploy.yml
       <br/>
Scaling container to five instances: <br/>
  kubectl scale --replicas=5 deployment/pizzabackend <br/>
The reason we need to specify deployment/pizzabackend instead of just pizzabackend is because we're scaling the entire Kubernetes deployment of the pizza backend service, and that will scale the instances of the individual pods correctly.<br/>

To scale the instance down, run: <br/>
    kubectl scale --replicas=1 deployment/pizzabackend



## Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
