# Stage 1: Build the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

# Copy the solution file and project files
COPY *.sln .
COPY ContactMATE.Api/*.csproj ./ContactMATE.Api/
COPY ContactMATE.BLL/*.csproj ./ContactMATE.BLL/
COPY ContactMATE.Core/*.csproj ./ContactMATE.Core/
COPY ContactMATE.Dal/*.csproj ./ContactMATE.Dal/

# Restore dependencies
RUN dotnet restore

# Copy the rest of the application code
COPY ContactMATE.Api/. ./ContactMATE.Api/
COPY ContactMATE.BLL/. ./ContactMATE.BLL/
COPY ContactMATE.Core/. ./ContactMATE.Core/
COPY ContactMATE.Dal/. ./ContactMATE.Dal/

# Build the application
WORKDIR /app/ContactMATE.Api
RUN dotnet publish -c Release -o out

# Stage 2: Create the runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/ContactMATE.Api/out ./

# Expose the port the app runs on (default for ASP.NET Core is 8080)
EXPOSE 8080

# Set the entry point to run the application
ENTRYPOINT ["dotnet", "ContactMATE.Api.dll"]