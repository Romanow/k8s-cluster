# kOps

```shell
$ tee .env >/dev/null <<EOT
export DIGITALOCEAN_ACCESS_TOKEN=<digitalocean access token>
export S3_ACCESS_KEY_ID=<s3 access key>
export S3_SECRET_ACCESS_KEY=<s3 access secret>

export S3_ENDPOINT=ams3.digitaloceanspaces.com
export KOPS_STATE_STORE=do://<s3 name>
EOT

$ . ./.env

$ kops create cluster \
  --cloud=digitalocean \
  --name=cluster.romanow-alex.ru \
  --image=ubuntu-22-04-x64 \
  --networking=calico \
  --ssh-public-key=~/.ssh/id_rsa.pub \
  --zones=ams3 \
  --node-size=s-2vcpu-4gb

$ kops update cluster cluster.romanow-alex.ru --yes

$ kops validate cluster --wait 10m

Using cluster from kubectl context: cluster.romanow-alex.ru

Validating cluster cluster.romanow-alex.ru

INSTANCE GROUPS
NAME                    ROLE            MACHINETYPE     MIN     MAX     SUBNETS
control-plane-ams3-1    ControlPlane    s-2vcpu-4gb     1       1       ams3
nodes-ams3              Node            s-2vcpu-4gb     1       1       ams3

NODE STATUS
NAME            ROLE            READY
10.110.0.2      control-plane   True
10.110.0.3      node            True

Your cluster cluster.romanow-alex.ru is ready

$ kubectl get nodes
NAME         STATUS   ROLES                AGE    VERSION
10.110.0.2   Ready    control-plane,node   3m3s   v1.26.5
10.110.0.3   Ready    node                 93s    v1.26.5

$ kops delete cluster --name cluster.romanow-alex.ru --yes
```

## Ссылки

1. [kOps](https://kops.sigs.k8s.io/)
2. [How to Create Spaces Bucket](https://docs.digitalocean.com/products/spaces/how-to/create/)
3. [How to Manage Administrative Access to Spaces](https://docs.digitalocean.com/products/spaces/how-to/manage-access/#access-keys)