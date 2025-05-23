FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY . .
WORKDIR /app/ContactMate.Api
RUN dotnet restore ContactMate.Api.csproj
RUN dotnet build ContactMate.Api.csproj -c Release -o /app/build
RUN dotnet publish ContactMate.Api.csproj -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "ContactMate.Api.dll"]