#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["gsop-logger-service/gsop-logger-service.csproj", "gsop-logger-service/"]
RUN dotnet restore "gsop-logger-service/gsop-logger-service.csproj"
COPY . .
WORKDIR "/src/gsop-logger-service"
RUN dotnet build "gsop-logger-service.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "gsop-logger-service.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "gsop-logger-service.dll"]