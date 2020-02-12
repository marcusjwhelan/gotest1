setting up with glcoud

```
// if project does not exist
gcloud projects create gotest1
// else
gcloud config set project gotest1
```

set set project to PROJECT ID
```
gcloud config set project PROJECT-ID
```

Launch deployment to google cloud COS container optimized OS
make sure to have
    * container_manifest.yaml
    * container_vm.py
    * container_vm.schema
    * container_vm.yaml

```
gcloud deployment-manager deployments create gotest1-deployment \
--config container_vm.yaml
```

Createas VM instance on google cloud with 
name: gotest1-deployment-gotest1-vm
gotetst1-deployment came from the create above and the gotest1-vm comes from container_vm.yaml file

Get a description of the instance
```
gcloud compute instances describe gotest1-deployment-gotest1-vm
```

Not able to view page yet. Need to create a firewall rule
```
gcloud compute firewall-rules create allow-80 --allow tcp:8080
```
Allows to go to external IP at ip:8080, not 80. need to figure out how to do this.

ssh into instance
```
gcloud compute ssh gotest1 --project ProjectID \
--zone ZONE
```

Can update firewall but it doesnt seem to allow, I think the manifest needs to change
```
gcloud compute firewall-rules update allow-80 --allow tcp:80
```

After you git push any changes need to update the image on the instance
```
gcloud compute instances update-container gotest1-deployment-gotest1-vm \
--container-image=mjwrazor/gotest1 \
--zone ZONE \
--container-restart-policy=always
```