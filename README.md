## Sample for the Build your first microservice in .NET learn module

Microservice applications are composed of small, independently versioned, and scalable customer-focused services that communicate with each other over standard protocols with well-defined interfaces. Each microservice typically encapsulates simple business logic, which you can scale out or in, test, deploy, and manage independently.  Smaller teams develop a microservice based on a customer scenario and use any technologies that they want to use. This module will teach you how to build your first microservice with .NET.

In order to run the code in this repo as a microservice, please complete the [Build your first microservice in .NET](https://docs.microsoft.com/learn/modules/dotnet-microservices).

### Steps to run

1. You will need to add the **Dockerfile** to the web API in the backend folder.
1. And add the **docker-compose.yml** file to the root folder.

Along with the necessary Docker commands for building an image and running Docker compose as well.

Check out the Learn module to find out all about [Building your first microservice in .NET](https://docs.microsoft.com/learn/modules/dotnet-microservices).

## Docker file

The following will be performed sequentially when Docker file is invoked:

Pull the mcr.microsoft.com/dotnet/sdk:6.0 image and name the image build
Set the working directory within the image to /src
Copy the file named backend.csproj found locally to the /src directory that was just created
Calls dotnet restore on the project
Copy everything in the local working directory to the image
Calls dotnet publish on the project
Pull the mcr.microsoft.com/dotnet/aspnet:6.0 image
Set the working directory within the image to /app
Exposes port 80 and 443
Copy everything from the /app directory of the build image created above into the app directory of this image
Sets the entrypoint of this image to dotnet and passes backend.dll as an argument

We've copied .csproj file separately and then ran dotnet restore, while dotnet publish command would have done it all. we've separated this to optimize
our image building process. Docker creates separate layer for each instruction in the file. The layers are stacked and each one is a delta of the 
changes from the previous layer. Docker also caches these layers so it doesn't hav to rebuild the layer when it is unchanged. That's why we've copied 
.csproj file and ran dotnet restore first. So all the packages are cached within that layer and they don't have to be relearn when we rebuild image.

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
