# Stage 1: Runtime Base
FROM mcr.microsoft.com/dotnet/aspnet:10.0-alpine AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

# Stage 2: SDK / Build & Restore
FROM mcr.microsoft.com/dotnet/sdk:10.0-alpine AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src


COPY ["Syncora.slnx", "./"]
COPY ["src/Syncora.Domain/Syncora.Domain.csproj", "src/Syncora.Domain/"]
COPY ["src/Syncora.Application/Syncora.Application.csproj", "src/Syncora.Application/"]
COPY ["src/Syncora.Infrastructure/Syncora.Infrastructure.csproj", "src/Syncora.Infrastructure/"]
COPY ["src/Syncora.WebAPI/Syncora.WebAPI.csproj", "src/Syncora.WebAPI/"]


RUN dotnet restore "Syncora.slnx"


COPY . .
WORKDIR "/src/src/Syncora.WebAPI"
RUN dotnet publish "Syncora.WebAPI.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Stage 3: Final Runner

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "Syncora.WebAPI.dll"]