FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src

# Copia apenas o arquivo de projeto e restaura as dependências
COPY ["StrictSensu.csproj", "./"]
RUN dotnet restore "StrictSensu.csproj"

# Copia o restante de todos os arquivos
COPY . .
RUN dotnet build "StrictSensu.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "StrictSensu.csproj" -c Release -o /app/publish /p:UseAppHost=false

# Gera a imagem final para rodar na Render
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app
EXPOSE 8080
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "StrictSensu.dll"]
