# New Service creation

After setting the initial part of the service on where you already have an stable server, the next step is to create the CI/CD pipe.

1. On root folder create folder .github/workflow
2. Create a file named `<service-name>-depl.yaml`
3. On that file put the following code:

```yaml
name: Build and Deploy to GKE <service-name> microservice

on:
  push:
    branches:
      - master

env:
  PROJECT_ID: ${{ secrets.GKE_PROJECT }}
  GKE_CLUSTER: amicasaio # cluster name
  GKE_ZONE: us-west2-a # cluster zone
  DEPLOYMENT_NAME: <service-name>-depl # deployment name
  IMAGE: <service-name> # docker image name

jobs:
  setup-build-publish-deploy:
    name: Setup, Build, Publish, and Deploy
    runs-on: ubuntu-latest

    # This action clones repo into this instance
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      # Setup gcloud CLI
      - name: Setup gcloud environment
        uses: GoogleCloudPlatform/github-actions/setup-gcloud@master
        with:
          service_account_key: ${{ secrets.GKE_SA_KEY }}
          project_id: ${{ secrets.GKE_PROJECT }}
          export_default_credentials: true
          service_account_email: ${{ secrets.GCP_SA_EMAIL }}

      # Configure Docker to use the gcloud command-line tool as a credential
      # helper for <service-name>entication
      - run: gcloud --quiet <service-name> configure-docker

      # Get the GKE credentials so we can deploy to the cluster
      - run: gcloud container clusters get-credentials "$GKE_CLUSTER" --zone us-west2-a

      # Build Docker image and push with google cloud build
      - name: Build
        run: gcloud builds submit --tag "gcr.io/$PROJECT_ID/$IMAGE"

      # Restart deployment rollout in the GKE cluster and ask for service status
      - name: Rollout deploy
        run: |-
          kubectl rollout restart deployment $DEPLOYMENT_NAME
          kubectl get services -o wide
```

4. Next we need to modify several infrastructure files regarding kubernetes. First we are going to see all the files regarding local development enviroment:

## `<service>-depl.yaml` k8s-dev

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <service-name>-depl
spec:
  replicas: 1
  selector:
    matchLabels:
      app: <service-name>
  template:
    metadata:
      labels:
        app: <service-name>
    spec:
      containers:
        - name: <service-name>
          image: amicasa/<service-name>
          env:
            - name: MONGO_URI
              value: 'mongodb://<service-name>-mongo-srv:27017/<service-name>'
            - name: AMICASA_NATS_URL
              value: 'http://amicasaio-nats-srv:4222'
            - name: AMICASA_NATS_CLUSTER_ID
              value: 'amicasaio-nats'
            - name: NODE_ENV
              value: 'DEVELOP'
            - name: AMICASA_NATS_CLIENT_ID
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: AMICASAIO_JWT_KEY
              valueFrom:
                secretKeyRef:
                  name: amicasaio-jwt-secret
                  key: AMICASAIO_JWT_KEY

---
apiVersion: v1
kind: Service
metadata:
  name: <service-name>-srv
spec:
  selector:
    app: <service-name>
  ports:
    - name: <service-name>
      protocol: TCP
      port: 3000
      targetPort: 3000
```

## `<service>-mongo-depl.yaml` k8s-dev

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <service-name>-mongo-depl
spec:
  replicas: 1
  selector:
    matchLabels:
      app: <service-name>-mongo
  template:
    metadata:
      labels:
        app: <service-name>-mongo
    spec:
      containers:
        - name: <service-name>-mongo
          image: mongo

---
apiVersion: v1
kind: Service
metadata:
  name: <service-name>-mongo-srv
spec:
  selector:
    app: <service-name>-mongo
  ports:
    - name: db
      protocol: TCP
      port: 27017
      targetPort: 27017
```

## Modify `ingress-srv.yaml` k8s-dev

Add under the paths field the following

```yaml
- path: /api/<service-name>/?(.*)
  backend:
    serviceName: <service-name>-srv
    servicePort: 3000
```

## Modify skaffold.yaml local enviroment

```yaml
- image: amicasa/<service-name>
      context: <service-name>
      docker:
        dockerfile: Dockerfile_dev
      sync:
        manual:
          - src: 'src/*/.ts'
            dest: .
```

5. Next we need to modify several infrastructure files regarding kubernetes. Now we are going to see all the files regarding production enviroment:

## `<service>-depl.yaml` k8s-prod

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <service-name>-depl
spec:
  replicas: 1
  selector:
    matchLabels:
      app: <service-name>
  template:
    metadata:
      labels:
        app: <service-name>
    spec:
      containers:
        - name: <service-name>
          image: gcr.io/amicasa-io/<service-name>
          env:
            - name: MONGO_URI
              value: 'mongodb://<service-name>-mongo-srv:27017/<service-name>'
            - name: AMICASA_NATS_URL
              value: 'http://amicasaio-nats-srv:4222'
            - name: AMICASA_NATS_CLUSTER_ID
              value: 'amicasaio-nats'
            - name: NODE_ENV
              value: 'DEVELOP'
            - name: AMICASA_NATS_CLIENT_ID
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: AMICASAIO_JWT_KEY
              valueFrom:
                secretKeyRef:
                  name: amicasaio-jwt-secret
                  key: AMICASAIO_JWT_KEY

---
apiVersion: v1
kind: Service
metadata:
  name: <service-name>-srv
spec:
  selector:
    app: <service-name>
  ports:
    - name: <service-name>
      protocol: TCP
      port: 3000
      targetPort: 3000
```

## `<service>-mongo-depl.yaml` k8s-dev

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <service-name>-mongo-depl
spec:
  replicas: 1
  selector:
    matchLabels:
      app: <service-name>-mongo
  template:
    metadata:
      labels:
        app: <service-name>-mongo
    spec:
      containers:
        - name: <service-name>-mongo
          image: mongo

---
apiVersion: v1
kind: Service
metadata:
  name: <service-name>-mongo-srv
spec:
  selector:
    app: <service-name>-mongo
  ports:
    - name: db
      protocol: TCP
      port: 27017
      targetPort: 27017
```

## Modify `ingress-srv.yaml`

Add under the 2 paths (www.amicasa.io and amicasa.io) field the following

```yaml
- path: /api/<service-name>/?(.*)
  backend:
    serviceName: <service-name>-srv
    servicePort: 3000
```

6. Push to master first the service so it can init its cd/ci pipe. ITS GOING TO FAIL, but we need to do this step so kubernetes has an image it can pull

7. Push to master de infrastructure so deployments are updated

8. Lastly push again the services repo to master, now, all should go well.
