FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release

COPY *.sln ./src/
COPY ./src/OrderService/OrderService.csproj ./src/OrderService/
WORKDIR app/src
RUN dotnet restore

COPY src/ ./src/
WORKDIR /app/src/OrderService
RUN dotnet build -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish -c $BUILD_CONFIGURATION -o /app/out /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/out .
ENTRYPOINT ["dotnet", "OrderService.dll"]