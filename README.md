# redis-deployment-through-helm-chart-on-gke


**Installing Redis on Google Kubernetes Engine (GKE) using Helm Chart**

Redis is a popular in-memory data store used for caching, session management, and as a message broker. Deploying Redis on Google Kubernetes Engine (GKE) using Helm Charts streamlines the installation process. Below are the steps to install Redis on GKE using Helm Chart:

**Step 1: Get the values.yaml file for Redis Helm Chart**

Run the following command to fetch the default values.yaml file for the Redis Helm Chart and save it as redis-values.yaml:

**helm show values oci://registry-1.docker.io/bitnami/redis > redis-values.yaml**

This file contains configuration options for Redis, including passwords, persistence settings, and resource requests/limits.

**Step 2: Customize Redis configuration**

Edit the redis-values.yaml file to customize the Redis configuration according to your requirements. You may want to change passwords, adjust resource requests, or enable/disable persistence based on your deployment needs.

**Step 3: Install Redis using Helm**

Execute the following command to install Redis on GKE using the customized values.yaml file:

**helm upgrade --install redis oci://registry-1.docker.io/bitnami/redis --values ./redis-values.yaml**

This command upgrades or installs the Redis Helm Chart with the provided configuration.

**Step 4: Expose Redis service**

To make Redis accessible externally, edit the Redis service:

**ubectl edit service/redis-master**

Add the following annotation to specify the load balancer type:

**annotations:**
  **networking.gke.io/load-balancer-type: External**

Additionally, ensure that the service type is set to LoadBalancer.

**Step 5: Verify Redis connection**

After exposing the Redis service, verify the connection using the following commands:

Telnet to the load balancer IP address:

**telnet <loadbalacer-ipaddress> 6379**

Ping Redis using redis-cli:

**redis-cli -h <loadbalacer-ipaddress> -p 6379 -a <redis password> ping**

Send a request to the Redis server:

**curl http://<loadbalacer-ipaddress>:<port>**

You should receive an empty response, indicating that the Redis server is accessible.

By following these steps, you can install and configure Redis on GKE using Helm Charts, making it easy to deploy and manage Redis clusters in a Kubernetes environment.
