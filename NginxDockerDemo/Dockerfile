# Use the official ASP.NET Core image as the base image
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

# Use the official build image to build the app
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["NginxDockerDemo/NginxDockerDemo.csproj", "NginxDockerDemo/"]
RUN dotnet restore "NginxDockerDemo/NginxDockerDemo.csproj"
COPY . .
WORKDIR "/src/NginxDockerDemo"
RUN dotnet build "NginxDockerDemo.csproj" -c Release -o /app/build

# Publish the app to the production image
FROM build AS publish
RUN dotnet publish "NginxDockerDemo.csproj" -c Release -o /app/publish

# Final stage, set the base image and copy the published files to it
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NginxDockerDemo.dll"]
