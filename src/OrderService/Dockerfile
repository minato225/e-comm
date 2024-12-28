FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
ARG BUILD_CONFIGURATION=Release

COPY .src/ .
RUN dotnet restore "./OrderService/OrderService.csproj"

COPY . .
RUN ls
WORKDIR "./OrderService"
RUN ls
RUN dotnet build "./OrderService.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./OrderService.csproj" -c $BUILD_CONFIGURATION -o /app/out /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/out .
ENTRYPOINT ["dotnet", "OrderService.dll"]