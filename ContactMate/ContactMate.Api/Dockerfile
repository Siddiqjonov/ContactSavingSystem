# Install clang/zlib1g-dev dependencies for publishing to native
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    clang zlib1g-dev

# Build Stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ContactMate.Api.csproj", "."]
COPY ["../ContactMate.Bll/ContactMate.Bll.csproj", "../ContactMate.Bll/"]
COPY ["../ContactMate.Core/ContactMate.Core.csproj", "../ContactMate.Core/"]
COPY ["../ContactMate.Dal/ContactMate.Dal.csproj", "../ContactMate.Dal/"]
RUN dotnet restore "ContactMate.Api.csproj"
COPY . .
WORKDIR /src
RUN dotnet build "ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish Stage
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final Stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ContactMate.Api.dll"]