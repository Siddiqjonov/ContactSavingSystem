# Install clang/zlib1g-dev dependencies for publishing to native
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    clang zlib1g-dev

# This stage is used to build the service project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ContactMate.Api/ContactMate.Api.csproj", "ContactMate.Api/"]
COPY ["ContactMate.Bll/ContactMate.Bll.csproj", "ContactMate.Bll/"]
COPY ["ContactMate.Core/ContactMate.Core.csproj", "ContactMate.Core/"]
COPY ["ContactMate.Dal/ContactMate.Dal.csproj", "ContactMate.Dal/"]
RUN dotnet restore "ContactMate.Api/ContactMate.Api.csproj"
COPY . ./src/ContactMate.Api/
WORKDIR "/src/ContactMate.Api"
RUN dotnet build "ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

# This stage is used to publish the service project to be copied to the final stage
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# This stage is used in production or when running from VS in regular mode (Default when not using the Debug configuration)
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ContactMate.Api.dll"]
