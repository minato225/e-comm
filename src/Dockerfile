FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /app

# Копируем csproj и восстанавливаем зависимости
COPY . .
WORKDIR /src
RUN dotnet restore

# Копируем весь код и собираем проект
COPY . .
WORKDIR /app/OrderService
RUN dotnet publish -c Release -o out

# Используем runtime образ для запуска
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS runtime
WORKDIR /app
COPY --from=build /app/OrderService/out ./
ENTRYPOINT ["dotnet", "OrderService.dll"]
