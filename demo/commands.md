build image and run container

```bash
# build image
docker build .

# view images id
docker images

# run container
docker run -p 8080:8080 af97fd910ebc
```

test (in other terminal)

```bash
curl localhost:8080
```

tag and publish image 

```bash
# set tag
docker tag af97fd910ebc ghcr.io/deniszm/demo:1.0.0

# securely set container registriy personal access toke to variable
read -s CR_PAT

# login to registry
echo $CR_PAT | docker login ghcr.io -u deniszm --password-stdin

# push image to registry
docker push ghcr.io/deniszm/demo:1.0.0
```

install KIND

```bash
go install sigs.k8s.io/kind@v0.27.0 && kind create cluster
kubectl version
alias k=kubectl
complete -F __start_kubectl k
k get all -A
```

deployment

```bash
kubectl create deployment demo --image ghcr.io/deniszm/demo:1.0.0

# add secret
k create secret docker-registry ghcr-secret --docker-server ghcr.io --docker-username deniszm --docker-password $CR_PAT

# bind secret to service account
k get serviceaccounts
k patch serviceaccounts default -p '{"imagePullSecrets": [{"name": "ghcr-secret"}]}'

# restart deployment
k rollout restart deployment demo

# watch pods
k get po -w
# view logs
k logs pods/demo-6945976954-rbwh9

# expose inside cluster / create ClusterIP service
k expose deployment demo --port 80 --target-port 8080
k get svc

# port-forware
 k port-forward svc/demo 8088:80
# in other terminal
 curl localhost:8088
```

inside pod/container

```bash
k exec -it demo-6945976954-rbwh9 -- sh

ls -l
ip a
ps ax
```