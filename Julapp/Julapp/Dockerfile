FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80


# Use the SDK to build the app
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Julapp.csproj", "./"]
RUN dotnet restore "./Julapp.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "Julapp.csproj" -c Release -o /app/build

# Publish the app
FROM build AS publish
RUN dotnet publish "Julapp.csproj" -c Release -o /app/publish

# Copy the published files to the base image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Julapp.dll"]
