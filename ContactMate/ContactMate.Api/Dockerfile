# Install clang/zlib1g-dev dependencies for publishing to native
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    clang zlib1g-dev

# Build Configuration=Release
WORKDIR /app
COPY ["ContactMate.Api.csproj", "./"]
RUN dotnet restore "ContactMate.Api.csproj"
COPY . .
WORKDIR "/app"
RUN dotnet build "ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ContactMate.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=true

# Final Stage
FROM mcr.microsoft.com/dotnet/runtime-deps:8.0 AS final
WORKDIR /app
EXPOSE 8080
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ContactMate.Api.dll"]