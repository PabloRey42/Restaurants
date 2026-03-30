FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Presentation/Restaurants.Presentation.Api/Restaurants.Presentation.Api.csproj", "Presentation/Restaurants.Presentation.Api/"]
COPY ["Infrastructure/Restaurants.Infrastructure.Application/Restaurants.Infrastructure.Application.csproj", "Infrastructure/Restaurants.Infrastructure.Application/"]
COPY ["Application/Restaurants.Application.Bussiness/Restaurants.Application.Bussiness.csproj", "Application/Restaurants.Application.Bussiness/"]
COPY ["Domain/Restaurants.Domain.Repositories/Restaurants.Domain.Repositories.csproj", "Domain/Restaurants.Domain.Repositories/"]
COPY ["Domain/Restaurants.Domain.Entities/Restaurants.Domain.Entities.csproj", "Domain/Restaurants.Domain.Entities/"]
COPY ["Domain/Restaurants.Domain.Exceptions/Restaurants.Domain.Exceptions.csproj", "Domain/Restaurants.Domain.Exceptions/"]
COPY ["Domain/Restaurants.Domain.Services/Restaurants.Domain.Services.csproj", "Domain/Restaurants.Domain.Services/"]
COPY ["Infrastructure/Restaurants.Infrastructure.Ioc/Restaurants.Infrastructure.Ioc.csproj", "Infrastructure/Restaurants.Infrastructure.Ioc/"]
COPY ["Infrastructure/Restaurants.Infrastructure.BlobStorage/Restaurants.Infrastructure.BlobStorage.csproj", "Infrastructure/Restaurants.Infrastructure.BlobStorage/"]
COPY ["Infrastructure/Restaurants.Infrastructure.Database/Restaurants.Infrastructure.Database.csproj", "Infrastructure/Restaurants.Infrastructure.Database/"]
COPY ["Application/Restaurant.Application.Ioc/Restaurant.Application.Ioc.csproj", "Application/Restaurant.Application.Ioc/"]
RUN dotnet restore "Presentation/Restaurants.Presentation.Api/Restaurants.Presentation.Api.csproj"
COPY . .
WORKDIR "/src/Presentation/Restaurants.Presentation.Api"
RUN dotnet build "Restaurants.Presentation.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Restaurants.Presentation.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Restaurants.Presentation.Api.dll"]
