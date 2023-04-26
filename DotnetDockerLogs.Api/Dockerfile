FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["DotnetDockerLogs.Api.csproj", "DotnetDockerLogs.Api/"]
RUN dotnet restore "DotnetDockerLogs.Api/DotnetDockerLogs.Api.csproj"
COPY . ./DotnetDockerLogs.Api
WORKDIR "/src/DotnetDockerLogs.Api"
RUN dotnet build "DotnetDockerLogs.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DotnetDockerLogs.Api.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
#RUN addgroup wheel && \
#    adduser --system --disabled-password --ingroup wheel deployer 
#USER deployer
#WORKDIR /home/deployer/app
#COPY --chown=deployer:wheel --from=publish /app/publish .
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DotnetDockerLogs.Api.dll"]
