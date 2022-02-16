## docker-compose example
docker-compose example utilizing traefik for reverse proxy. Uses environment variables to pass in parameters on docker-compose up.

To see docker-compose version with parameters added you can run.
```
docker-compose config
```

Export Postgresql password to environment variable to be included in the docker-file
```
export POSTGRES_PASSWORD=secret
```

Verify docker-compose is reading the POSTGRES_PASSWORD correctly
```
docker-compose config
```

Create resources specified in docker-compose file with detached session.
```
docker-compose up -d
```

Destroy all resources that were ( including networks and volumes )
```
docker-compose down -v
```

Remove all remaining images, don't ask for prompt.
```
docker image prune -f
```
