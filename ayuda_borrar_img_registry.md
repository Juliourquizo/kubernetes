```bash
curl http://192.168.39.178:5000/v2/_catalog
curl http://192.168.39.178:5000/v2/<name>/tags/list
curl -v --silent -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X GET http://192.168.39.178:5000/v2/<name>/manifests/latest 2>&1 | grep Docker-Content-Digest | awk '{print ($3)}'
curl -v -H "Accept: application/vnd.docker.distribution.manifest.v2+json" -X DELETE http://192.168.39.178:5000/v2/<name>/manifests/sha256:72efe8582aebe01c94c0b7b778fe65595633b19a11b2b8912146d7ff4d1e7f15
``` 


```bash
eval $(minikube docker-env)
docker exec -it 80f50e8606c7 /bin/sh
bin/registry garbage-collect kk.yaml -m
```

`kk.yaml`

```yaml
version: 0.1
log:
  fields:
  service: registry
storage:
    cache:
        blobdescriptor: inmemory
    filesystem:
        rootdirectory: /var/lib/registry
    delete:
        enabled: true
http:
    addr: :5000
    headers:
        X-Content-Type-Options: [nosniff]
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
```