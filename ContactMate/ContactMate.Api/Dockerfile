# Install clang/zlib1g-dev dependencies for publishing to native
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    clang zlib1g-dev

# Build Stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY . .
WORKDIR /app/ContactMate.Api
RUN dotnet restore "ContactMate.Api.csproj"
RUN dotnet build "ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish Stage
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=true

# Final Stage
FROM mcr.microsoft.com/dotnet/runtime-deps:8.0 AS final
WORKDIR /app
EXPOSE 8080
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "ContactMate.Api.dll"]