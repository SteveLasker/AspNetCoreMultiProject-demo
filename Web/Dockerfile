FROM microsoft/aspnetcore:2.0 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /src
COPY *.sln ./
COPY Web/Web.csproj Web/
COPY Models/Models.csproj Models/
COPY Data/Data.csproj Data/
RUN dotnet restore
COPY . .
WORKDIR /src/Web
RUN dotnet build -c Release -o /app

FROM build AS test
WORKDIR /src/Web.test
RUN dotnet test

FROM build AS publish
RUN dotnet publish -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Web.dll"]
