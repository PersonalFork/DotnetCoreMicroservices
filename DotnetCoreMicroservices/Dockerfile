#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["DotnetCoreMicroservices/DotnetCoreMicroservices.csproj", "DotnetCoreMicroservices/"]
RUN dotnet restore "DotnetCoreMicroservices/DotnetCoreMicroservices.csproj"
COPY . .
WORKDIR "/src/DotnetCoreMicroservices"
RUN dotnet build "DotnetCoreMicroservices.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DotnetCoreMicroservices.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DotnetCoreMicroservices.dll"]