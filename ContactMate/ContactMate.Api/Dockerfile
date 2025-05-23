# Base image for building
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

# Install clang/zlib1g-dev dependencies for publishing to native
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    clang zlib1g-dev

# Set working directory in the container
WORKDIR /app

# Copy the ContactMate.Api directory from the build context (repo root)
COPY ContactMate.Api ContactMate.Api

# Change to the project directory
WORKDIR /app/ContactMate.Api

# Restore dependencies
RUN dotnet restore ContactMate.Api.csproj

# Build the project
ARG BUILD_CONFIGURATION=Release
RUN dotnet build ContactMate.Api.csproj -c $BUILD_CONFIGURATION -o /app/build

# Publish the project
RUN dotnet publish ContactMate.Api.csproj -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=true

# Final Stage: Runtime image
FROM mcr.microsoft.com/dotnet/runtime-deps:8.0 AS final
WORKDIR /app
EXPOSE 8080
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "ContactMate.Api.dll"]