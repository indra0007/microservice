#### Run docker-compose to start frontend/backend/database

This command will start up all three components:
 
 * frontend
 * backend
 * postgres database


```
docker-compose up
```

#### Run postgres instance

```bash
docker rmi $(docker images -q) -f
docker ps --filter status=dead --filter status=exited -aq | xargs -r docker rm -v
docker run --name micro-postgres -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=password -p 5432:5432 -d postgres
a138091fa815a92d000bf0defaea791acc8daff5d22309289feee120589fd050

psql --host=localhost --port=5432 -U postgres

```          

#### Create database microservice

```sql
psql --host=localhost --port=5432 -U postgres # password
CREATE DATABASE  microservice;
CREATE USER micro WITH ENCRYPTED PASSWORD 'password'; 
GRANT ALL PRIVILEGES ON DATABASE microservice TO micro;
ALTER DATABASE microservice OWNER TO micro;
```

#### Connect to database
```sql
psql --host=localhost --port=5432 -U micro -d microservice
select * from request_ips;


```
#### Enviromental variables 
```bash
PSQL_DB_USER        default='micro'
PSQL_DB_PASS        default='password'
PSQL_DB_NAME        default='microservice'
PSQL_DB_ADDRESS     default='127.0.0.1'
PSQL_DB_PORT        default='5432'
```

#### Run microservice app
```bash
docker rm micro-service
docker build -t microservice:v0.0.1 .
docker run --rm --name micro-service -it -e PSQL_DB_ADDRESS=192.168.1.45 -p 5001:8000 -d microservice:v0.0.1

```



#### Articles to read
https://kubernetes.io/docs/tasks/access-application-cluster/connecting-frontend-backend/
https://mherman.org/blog/dockerizing-a-react-app/
https://medium.com/greedygame-engineering/so-you-want-to-dockerize-your-react-app-64fbbb74c217

#### helm chart backend/frontend

```
helm create micro-service
cd micro-service

cat <<EOF > requirements.yaml
dependencies:
- name: postgresql
  repository: https://kubernetes-charts.storage.googleapis.com
  version: 3.18.3

EOF

helm dependency update

```
