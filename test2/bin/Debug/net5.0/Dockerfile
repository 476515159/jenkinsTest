FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim as build-env
WORKDIR /app
# 拷贝所有文件到环境中,并进行restore,build and release
COPY . ./
RUN dotnet restore
RUN dotnet build test2.csproj
RUN dotnet publish test2.csproj -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim
WORKDIR /app
#把发布好的文件拷贝到镜像中
COPY --from=build-env /app/out ./ 

EXPOSE 80 
ENTRYPOINT [ "dotnet", "test2.dll" ]
