![ASP.NET Docker Image](https://avatars2.githubusercontent.com/u/6154722?v=3&s=200)
# ASP.NET Docker Image

NOTE: This is a Windows container image. [Learn more about Windows containers](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/about/about_overview).

## Supported tags and respective `Dockerfile` links

* 4.6.2, latest ([4.6.2/Dockerfile](https://github.com/microsoft/aspnet-docker/blob/master/4.6.2/Dockerfile))
* 3.5 ([3.5/Dockerfile](https://github.com/microsoft/aspnet-docker/blob/master/3.5/Dockerfile))

This image is built from the [microsoft/aspnet-docker GitHub repo](https://github.com/microsoft/aspnet-docker).

## What is ASP.NET?
ASP.NET is a high productivity  framework for building Web Applications using Web Forms, MVC, Web API and SignalR.

## How to use this image?
### Create a Dockerfile with your website
```
FROM microsoft/aspnet

RUN mkdir C:\aspnet

RUN powershell -NoProfile -Command \
    Import-module IISAdministration; \
    New-IISSite -Name "ASPNET" -PhysicalPath C:\aspnet -BindingInformation "*:8000:"

EXPOSE 8000

ADD . /aspnet
```
You can then build and run the Docker image:
```
$ docker build -t iis-site .
$ docker run -d -p 8000:8000 --name my-running-site iis-site
```

There is no need to specify an `ENTRYPOINT` in your Dockerfile since the `microsoft/aspnet` base image already includes an entrypoint application that monitors the status of the IIS World Wide Web Publishing Service (W3SVC).

### Verify in the browser

> With the current release, you can't use `http://localhost` to browse your site from the container host. This is because of a known behavior in WinNAT, and will be resolved in future. Until that is addressed, you need to use the IP address of the container.

Once the container starts, you'll need to finds its IP address so that you can connect to your running container from a browser. You use the `docker exec` command to do that:

`docker exec my-running-site ipconfig`

You will see an output similar to this:

```
Windows IP Configuration


Ethernet adapter Ethernet 2:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::91d:ce97:dd27:460d%5
   IPv4 Address. . . . . . . . . . . : 172.31.194.61
   Subnet Mask . . . . . . . . . . . : 255.240.0.0
   Default Gateway . . . . . . . . . : 172.16.0.1
```

You can connect the running container using the IP address and configured port, `http://172.31.194.61:8000` in the example shown.

For a comprehensive tutorial on running an ASP.NET app in a container, check out [the tutorial on the docs site](https://docs.microsoft.com/en-us/dotnet/articles/framework/docker/aspnetmvc).

## Supported Docker Versions
This image has been tested on Docker Versions 1.12.1-beta26 or higher.

## License of this repository

MIT

## License of ASP.NET (.NET Framework) images on Dockerhub 

MICROSOFT SOFTWARE SUPPLEMENTAL LICENSE TERMS

CONTAINER OS IMAGE

Microsoft Corporation (or based on where you live, one of its affiliates) (referenced as “us,” “we,” or “Microsoft”) licenses this Container OS Image supplement to you (“Supplement”). You are licensed to use this Supplement in conjunction with the underlying host operating system software (“Host Software”) solely to assist running the containers feature in the Host Software. The Host Software license terms apply to your use of the Supplement. You may not use it if you do not have a license for the Host Software. You may use this Supplement with each validly licensed copy of the Host Software.
## User Feedback
If you have any issues or concerns, reach out to us through a [GitHub issue](https://github.com/Microsoft/aspnet-docker/issues/new).
