# Stage 1: Base Image
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Stage 2: Build Image
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy the project file into the container
COPY devops.fusion.webapi.csproj .
RUN dotnet restore devops.fusion.webapi.csproj

# Copy all files to the container
COPY . .
WORKDIR /src

# Build the application
RUN dotnet build devops.fusion.webapi.csproj -c $BUILD_CONFIGURATION -o /app/build

# Stage 3: Publish Image
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish devops.fusion.webapi.csproj -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Stage 4: Final Image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "devops.fusion.webapi.dll"]