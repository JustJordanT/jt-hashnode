# Day 8: Using Docker for DevOps

Docker is an essential tool for development and deployment in a DevOps workflow. It allows developers to package applications in containers, which are lightweight, standalone, and executable packages that contain everything an application needs to run, including code, libraries, dependencies, and runtime.

Using Docker in a DevOps workflow can provide several benefits:

**Consistency**: Docker ensures that an application runs the same way on any infrastructure, regardless of the environment it is deployed to. This helps to prevent discrepancies between development, staging, and production environments.

**Portability**: Docker containers can be easily moved between different environments and machines, making it easy to deploy applications anywhere.

**Isolation**: Containers provide a level of isolation between applications, allowing multiple applications to run on the same host without affecting each other.

**Efficiency**: Docker allows developers to use fewer resources, as containers share the host operating system and kernel, rather than running a full copy of an operating system for each application.

To use Docker in a DevOps workflow, developers can create a Dockerfile that specifies the instructions for building a Docker image for their application. This image can then be stored in a Docker registry and used to deploy the application to any environment.

Here is an example of a `Dockerfile`

```plaintext
FROM mcr.microsoft.com/dotnet/aspnet:6.0-alpine AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0-alpine AS build
WORKDIR /src
COPY ./WebApp1.csproj .
RUN dotnet restore
COPY . .
WORKDIR "/src/"
RUN dotnet build "WebApp1.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "WebApp1.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebApp1.dll"]
```

Using a `Dockerfile` is how we can get our source code into a working service where we can take this image and host it on a cloud service of our choice. ie, AWS, GCP, Azure

![Docker Image | A Complete Guide For Beginners | K21 Academy](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fcdn.shortpixel.ai%2Fclient%2Fq_glossy%2Cret_img%2Cw_840%2Ch_293%2Fhttps%3A%2F%2Fk21academy.com%2Fwp-content%2Fuploads%2F2020%2F06%2FDockerFile_Diagram-min.png&f=1&nofb=1&ipt=2f392deaf5b9420585c01bee629dbc6284154624ccea3a87945dd33559587f5e&ipo=images align="left")

DevOps teams can use tools such as Docker Compose to define and manage multi-container applications and use continuous integration and deployment (CI/CD) tools such as Jenkins to automate the build, test, and deployment of Docker containers.

## Resources

[https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

[https://docs.docker.com/registry/](https://docs.docker.com/registry/)

[https://aws.amazon.com/docker/](https://aws.amazon.com/docker/)

[https://aws.amazon.com/getting-started/hands-on/deploy-docker-containers/](https://aws.amazon.com/getting-started/hands-on/deploy-docker-containers/)