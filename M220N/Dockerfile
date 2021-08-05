#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /app

RUN curl -sL https://deb.nodesource.com/setup_10.x |  bash -
RUN apt-get install -y nodejs

COPY . ./
RUN dotnet restore
COPY . ./

# testing
FROM build AS testing
WORKDIR /app
RUN dotnet build
WORKDIR /app
RUN dotnet test

# Build
WORKDIR /app
RUN dotnet publish -c release -o /app/publish --no-cache

#Get the runtime into a folder called app
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS runtime
WORKDIR /app

# COPY --from=publish /app/M220N.db .
COPY --from=publish /app/publish .
# ENTRYPOINT ["dotnet", "M220N.dll"]
# Change to heroku entry point
CMD ASPNETCORE_URLS=http://*:$PORT dotnet M220N.dll