#FROM mcr.microsoft.com/dotnet/aspnet:5.0 as build-env
#WORKDIR /app
## 拷贝所有文件到环境中,并进行restore,build and release
#COPY . ./
#RUN dotnet restore
#RUN dotnet build test2.csproj
#RUN dotnet publish test2.csproj -c Release -o out
#
#FROM microsoft/dotnet:2.2-aspnetcore-runtime
#WORKDIR /app
##把发布好的文件拷贝到镜像中
#COPY --from=build-env /app/out ./ 
#
#EXPOSE 80 
#ENTRYPOINT [ "dotnet", "test2.dll" ]


FROM mcr.microsoft.com/dotnet/aspnet:5.0-alpine AS base
WORKDIR /app
EXPOSE 5000

FROM mcr.microsoft.com/dotnet/sdk:5.0-alpine AS build
WORKDIR /src
COPY ["test2/test2.csproj", "test2/"]
RUN dotnet restore "test2/test2.csproj"
COPY . .
WORKDIR "/src/test2"
RUN dotnet build "test2.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "test2.csproj" -c Release -o /app/publish \
    --runtime alpine-x64 \
    --self-contained true \
    /p:PublishTrimmed=true \
    /p:PublishSingleFile=true

FROM mcr.microsoft.com/dotnet/runtime-deps:5.0-alpine AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["./test2"]
