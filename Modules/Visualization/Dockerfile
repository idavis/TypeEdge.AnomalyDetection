ARG base_tag=2.1.0-runtime-bionic
FROM microsoft/dotnet:${base_tag} AS base
WORKDIR /app

FROM microsoft/dotnet:2.1-sdk AS build
WORKDIR /src
COPY Modules/Visualization/Visualization.csproj Modules/Visualization/
COPY Modules/VisualizationWeb/VisualizationWeb.csproj Modules/VisualizationWeb/

COPY Thermostat.Shared/Thermostat.Shared.csproj Thermostat.Shared/
COPY NuGet.Config ./
RUN dotnet restore Modules/Visualization/Visualization.csproj
COPY . . 
WORKDIR /src/Modules/Visualization
RUN dotnet build Visualization.csproj -c Release -o /app
  
FROM build AS publish
RUN dotnet publish Visualization.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Visualization.dll"]
 