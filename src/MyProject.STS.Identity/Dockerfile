FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG TARGETARCH
WORKDIR /src
COPY ["src/MyProject.STS.Identity/MyProject.STS.Identity.csproj", "src/MyProject.STS.Identity/"]
COPY ["src/MyProject.Admin.EntityFramework.Shared/MyProject.Admin.EntityFramework.Shared.csproj", "src/MyProject.Admin.EntityFramework.Shared/"]
COPY ["src/MyProject.Shared/MyProject.Shared.csproj", "src/MyProject.Shared/"]
RUN dotnet restore -a $TARGETARCH "src/MyProject.STS.Identity/MyProject.STS.Identity.csproj"
COPY . .
WORKDIR "/src/src/MyProject.STS.Identity"
RUN dotnet build -a $TARGETARCH "MyProject.STS.Identity.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -a $TARGETARCH "MyProject.STS.Identity.csproj" -c Release --no-restore -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENV ASPNETCORE_FORWARDEDHEADERS_ENABLED=true
ENTRYPOINT ["dotnet", "MyProject.STS.Identity.dll"]