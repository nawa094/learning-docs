# Use the official .NET runtime image as the base image
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80

# Use the official .NET SDK image to build the application
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["src/DotNetWebApi/DotNetWebApi.csproj", "DotNetWebApi/"]
RUN dotnet restore "DotNetWebApi/DotNetWebApi.csproj"

# Copy the application code and build it
COPY ["src/DotNetWebApi/", "DotNetWebApi/"]
WORKDIR "/src/DotNetWebApi"
RUN dotnet build "DotNetWebApi.csproj" -c Release -o /app/build

# Publish the application
FROM build AS publish
RUN dotnet publish "DotNetWebApi.csproj" -c Release -o /app/publish

# Create the final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DotNetWebApi.dll"]