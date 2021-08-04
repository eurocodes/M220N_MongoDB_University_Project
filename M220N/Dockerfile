#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /app
COPY . ./
RUN dotnet restore
COPY . ./

# testing
FROM build AS testing
WORKDIR /app
RUN dotnet build
WORKDIR /app
RUN dotnet test

# Install nodejs
FROM Node
# Create app directory
WORKDIR /app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . ./

# Build
WORKDIR /app
RUN dotnet build

# Publishing
FROM build AS publish
WORKDIR /app
RUN dotnet publish -c Release -o /app/publish

#Get the runtime into a folder called app
FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS runtime
WORKDIR /app

# Expose port 80 to your local machine so you can access the app.
EXPOSE 80
# COPY --from=publish /app/M220N.db .
COPY --from=publish /app/publish .
# ENTRYPOINT ["dotnet", "M220N.dll"]
# Change to heroku entry point
CMD ASPNETCORE_URLS=http://*:$PORT dotnet M220N.dll