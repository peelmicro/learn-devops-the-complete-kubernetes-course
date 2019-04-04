## Retrieve keys from kops

```
aws s3 sync s3://kubernetes.peelmicro.com/kubernetes.peelmicro.com/pki/private/ca/ ca-key
aws s3 sync s3://kubernetes.peelmicro.com/kubernetes.peelmicro.com/pki/issued/ca/ ca-crt
mv ca-key/*.key ca.key
mv ca-crt/*.crt ca.crt
```

## Create new user

```
sudo apt install openssl
openssl genrsa -out juan.pem 2048
openssl req -new -key juan.pem -out juan-csr.pem -subj "/CN=juan/O=myteam/"
openssl x509 -req -in juan-csr.pem -CA ca.crt -CAkey ca.key -CAcreateserial -out juan.crt -days 10000
```

## add new context

```
kubectl config set-credentials juan --client-certificate=juan.crt --client-key=juan.pem
kubectl config set-context juan --cluster=kubernetes.peelmicro.com --user juan
```
