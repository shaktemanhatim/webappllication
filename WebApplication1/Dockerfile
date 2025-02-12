# Use the official ASP.NET image for runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app

# Expose port 5052 for your application
EXPOSE 5052

# Use the official .NET SDK image for building the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy the project file to restore dependencies
COPY WebApplication1/WebApplication1.csproj WebApplication1/
RUN dotnet restore "./WebApplication1/WebApplication1.csproj"

# Copy the rest of the application files and build the application
COPY . .
WORKDIR "/src/WebApplication1"
RUN dotnet build "./WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish the application for deployment
FROM build AS publish
RUN dotnet publish "./WebApplication1.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final image to run the application in production
FROM base AS final
WORKDIR /app

# Copy the published files from the publish stage
COPY --from=publish /app/publish .

# Set the entry point to run the application
ENTRYPOINT ["dotnet", "WebApplication1.dll"]
