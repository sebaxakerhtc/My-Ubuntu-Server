### Nextcloud VM
```
docker stop onlyoffice

docker system prune -a

docker run -i -t -d -p 127.0.0.3:9090:80 -e JWT_ENABLED=true -e JWT_HEADER=AuthorizationJwt -e JWT_SECRET="MyStrongPassword" --restart always --name onlyoffice onlyoffice/documentserver
```
