FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["StudentAffairsWebApp/StudentAffairsWebApp/StudentAffairsWebApp.csproj", "StudentAffairsWebApp/StudentAffairsWebApp/"]
COPY ["StudentAffairsWebApp/StudentAffairsWebApp.Client/StudentAffairsWebApp.Client.csproj", "StudentAffairsWebApp/StudentAffairsWebApp.Client/"]
RUN dotnet restore "StudentAffairsWebApp/StudentAffairsWebApp/StudentAffairsWebApp.csproj"
COPY . .
WORKDIR "/src/StudentAffairsWebApp/StudentAffairsWebApp"
RUN dotnet build "StudentAffairsWebApp.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "StudentAffairsWebApp.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "StudentAffairsWebApp.dll"]
