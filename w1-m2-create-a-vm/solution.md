## M2-W1: Compute Engine Foundations: Creating & Configuring a Virtual Machine

### Set the project, region, zone & instance name
```red
gcloud config set project women-tech-series-2
```

```
gcloud config set compute/region us-central1
export REGION=$(gcloud config get compute/region)
```

```
gcloud config set compute/zone us-central1-a
export ZONE=$(gcloud config get compute/zone)
```

```
export INSTANCE_NAME=wts-server-2
```

### Task1: Create VM instance manually.
- Use the Google Cloud Console ( follow the Cloud Skills Boost )



### Task 2: Create a new instance with gcloud

1. Create VM
```
gcloud compute instances create $INSTANCE_NAME \
    --zone=$ZONE \
    --machine-type=e2-medium
```

2. Attach a network tag to your running vm
```
gcloud compute instances add-tags $INSTANCE_NAME \
    --zone=$ZONE  \
    --tags=nginx-server
```

3. Add a firewall to allow access the server on port 80
```
gcloud compute firewall-rules create http-allow-access-nginx-server \
    --allow=tcp:80 \
    --target-tags=nginx-server
```



### Task 3: Install NGINX web server

1. SSH to connect into your instance
```
gcloud compute ssh $INSTANCE_NAME
```
2. First update the OS:
```
sudo apt-get update
```

3. Install Nginx:
```
sudo apt-get install nginx -y
```

4. Confirm that NGINX is running:
```
ps auwx | grep nginx
```

5. Exit the server shell, back to the project shell
```
exit
```

5. View the external IP for a specific instance
```
gcloud compute instances describe $INSTANCE_NAME \
    --zone=$ZONE \
    --format='get(networkInterfaces[0].accessConfigs[0].natIP)'
```

6. Copy the above vm external IP and access the webpage in a browser
```
http://[EXTERNAL_IP]
```

### Task 4: Delete the resources

1. Delete the vm, when prompt type: `Y`
```
gcloud compute instances delete $INSTANCE_NAME --zone=$ZONE
``` 

2. Delete the firewall rule
```
gcloud compute firewall-rules delete http-allow-access-nginx-server
```

### Congratulations
You created a VM with Compute Engine and performed a few configurations.