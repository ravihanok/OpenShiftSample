# Use a Linux-based .NET runtime image as the base
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Use a Linux-based .NET SDK image for building the app
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
RUN mkdir -p /src/dependencies
WORKDIR /src

# Copy .csproj file and restore dependencies
# RUN mkdir -p ./dependencies
# COPY OpenShiftSample.csproj ./dependencies  # Correct path
COPY . .

# Ensure the destination is a directory and run dotnet restore
RUN dotnet restore "OpenShiftSample.csproj"

# Copy the rest of the files
# COPY ./ .  # Copy the remaining source code

# Build the project
WORKDIR "/src"
RUN dotnet build "OpenShiftSample.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Publish the app
FROM build AS publish
RUN dotnet publish "OpenShiftSample.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final image with .NET runtime, ready to run the app
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "OpenShiftSample.dll"]
