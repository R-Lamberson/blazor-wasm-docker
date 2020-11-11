#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# -- this fails
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-nanoserver-1809 AS base 
# -- this works
# FROM mcr.microsoft.com/dotnet/aspnet:3.1.10-buster-slim AS base

WORKDIR /app
# HTTP
EXPOSE 80
# HTTPS
EXPOSE 443

# -- this fails
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-nanoserver-1809 AS build
# -- this works
# FROM mcr.microsoft.com/dotnet/sdk:3.1.404-buster AS build 
WORKDIR /src

COPY ["./app/Server/BlazorAppWasm.Server.csproj", "./Server/BlazorAppWasm.Server.csproj"]
COPY ["./app/Client/BlazorAppWasm.Client.csproj", "./Client/BlazorAppWasm.Client.csproj"]
COPY ["./app/Shared/BlazorAppWasm.Shared.csproj", "./Shared/BlazorAppWasm.Shared.csproj"]

RUN dotnet restore "./Server/BlazorAppWasm.Server.csproj"

COPY ./app/ ./

RUN dotnet build "./Server/BlazorAppWasm.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "./Server/BlazorAppWasm.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorAppWasm.Server.dll"]