# Install the Gateway API resources

kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v1.5.1" | kubectl apply -f -

# Deploy the NGINX Gateway Fabric CRDs

kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/crds.yaml

# Deploy NGINX Gateway Fabric

kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/nodeport/deploy.yaml

# Verify the Deployment

kubectl get pods -n nginx-gateway

# View the nginx-gateway service

kubectl get svc -n nginx-gateway nginx-gateway -o yaml

# Update the nginx-gateway service to expose ports 30080 for HTTP and 30081 for HTTPS

kubectl patch svc nginx-gateway -n nginx-gateway --type='json' -p='[
{"op": "replace", "path": "/spec/ports/0/nodePort", "value": 30080},
{"op": "replace", "path": "/spec/ports/1/nodePort", "value": 30081}
]'
