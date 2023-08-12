#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["DockerNoin/Server/DockerNoin.Server.csproj", "DockerNoin/Server/"]
COPY ["DockerNoin/Client/DockerNoin.Client.csproj", "DockerNoin/Client/"]
COPY ["DockerNoin/Shared/DockerNoin.Shared.csproj", "DockerNoin/Shared/"]
RUN dotnet restore "DockerNoin/Server/DockerNoin.Server.csproj"
COPY . .
WORKDIR "/src/DockerNoin/Server"
RUN dotnet build "DockerNoin.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DockerNoin.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DockerNoin.Server.dll"]