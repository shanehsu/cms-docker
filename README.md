# 操作步驟

1. 建立內部網路

docker network create --attachable --internal cms-network

2. 開資料庫

docker run --network cms-network --name postgres -e POSTGRES_USER=cmsuser -e POSTGRES_DB=cmsdb -e POSTGRES_PASSWORD=PASSW0RD -p 5432:5432 -d postgres

3. 建置 CMS 映像

docker build --tag cms --file Dockerfiles/Dockerfile .

4. 執行 cmsInitDB

docker run --network cms-network -it --rm cms cmsInitDB
docker run --network cms-network -it --rm cms cmsAddAdmin shanehsu

會產生帳號、密碼。

5. 執行 cmsAdminWebServer 並建立競賽

docker run -p 8080:8080 --network cms-network --name aws -it --rm cms cmsAdminWebServer
在另一個 Terminal：
docker network connect bridge aws

進到 localhost:8080 中，建立競賽，關掉兩個 Terminal。

6. 執行所有服務

docker run -p 8080:8080 -p 80:80 -P --network cms-network --name cms -it --rm cms
在另一個 Terminal：
docker network connect bridge cms

後面那個 Terminal 可以關掉
