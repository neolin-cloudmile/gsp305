# gsp305

1. Build and deploy the updated application with a new tag
   ```
   gsutil cp gs://qwiklabs-gcp-fd350320cc8c07d6/echo-web-v2.tar.gz 
   tar zxf echo-web-v2.tar.gz
   export PROJECT_ID="$(gcloud config get-value project -q)"
   docker build -t gcr.io/${PROJECT_ID}/echo-app:v2 .
   gcloud docker -- push gcr.io/${PROJECT_ID}/echo-app:v2
   ```
2. Push the image to the Google Container Registry
   ```
   gcloud docker -- push gcr.io/${PROJECT_ID}/echo-app:v2
   ```
3. Confirm current the web version
   ```
   curl http://35.222.204.105/
   ```
   ```
   Echo Test
   Version: 1.0.0
   Hostname: echo-web-5798dfc4c4-gq5c4
   Host ip-address(es): 10.40.0.11
   ```
4. The echo-app application in the echo-web deployment from the v1 to the v2
   ```
   source <(kubectl completion bash)
   gcloud container clusters get-credentials echo-cluster --zone us-central1-a
   kubectl edit deployment echo-web
   ```
   ```
    32      spec:
    33        containers:
    34        - image: gcr.io/qwiklabs-gcp-fd350320cc8c07d6/echo-app:v2 #Change the v1 to v2
    35          imagePullPolicy: IfNotPresent
    36          name: echo-web
   ```
5. Confirm current the web version
   ```
   curl http://35.222.204.105/
   ```
   ```
   Echo Test
   Version: 2.0.0
   Hostname: echo-web-5798dfc4c4-gq5c4
   Host ip-address(es): 10.40.0.11
   ```
6. Scale out the application to 2 instances
   ```
   kubectl scale deployment echo-web --replicas=2
   kubectl get pods
   ```
