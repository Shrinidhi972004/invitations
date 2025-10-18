# Pooja Invitation - Production Kubernetes Deployment

Complete production-ready deployment for your Pooja invitation with auto-scaling.

## 🚀 Quick Deploy (Production Ready)

### 1. Prerequisites
```bash
# Ensure you have kubectl configured for your cluster
kubectl cluster-info

# Verify nodes are ready
kubectl get nodes
```

### 2. Build & Deploy
```bash
# Build Docker image
docker build -t pooja-invitation:latest .

# For KIND clusters
kind load docker-image pooja-invitation:latest --name your-cluster-name

# For cloud clusters, push to registry
# docker tag pooja-invitation:latest your-registry/pooja-invitation:latest
# docker push your-registry/pooja-invitation:latest
```

### 3. Deploy to Kubernetes
```bash
# Deploy in order
kubectl apply -f k8s-namespace.yaml
kubectl apply -f k8s-deployment.yaml
kubectl apply -f k8s-hpa.yaml
kubectl apply -f k8s-ingress.yaml
```

### 4. Verify Deployment
```bash
# Check all resources
kubectl get all -n pooja-invitation

# Expected output:
# - 2 pods running
# - 1 service (ClusterIP)
# - 1 deployment
# - 1 HPA (ready)
# - 1 ingress (with external IP)
```

## 📊 Monitoring & Status

### Real-time Monitoring
```bash
# Watch pods scaling
kubectl get pods -n pooja-invitation -w

# Monitor HPA status
kubectl get hpa -n pooja-invitation -w

# Check service endpoints
kubectl get svc -n pooja-invitation

# View ingress status
kubectl get ingress -n pooja-invitation
```

### Detailed Status
```bash
# Pod details and logs
kubectl describe pods -n pooja-invitation
kubectl logs -l app=pooja-invitation -n pooja-invitation

# HPA metrics
kubectl describe hpa pooja-invitation-hpa -n pooja-invitation

# Service connectivity
kubectl port-forward svc/pooja-invitation-service 8080:80 -n pooja-invitation
```

## 🔧 Access Methods

### Method 1: Domain Access (Production)
- **URL:** http://pooja.shrinidhi.space
- **Requirements:** Domain configured, nginx-ingress controller installed
- **Best for:** Public access, sharing with guests

### Method 2: Port Forward (Testing)
```bash
kubectl port-forward svc/pooja-invitation-service 8080:80 -n pooja-invitation
```
Access at: http://localhost:8080

### Method 3: LoadBalancer (Cloud)
```bash
# Change service type to LoadBalancer
kubectl patch svc pooja-invitation-service -n pooja-invitation -p '{"spec":{"type":"LoadBalancer"}}'

# Get external IP
kubectl get svc -n pooja-invitation
```

## ⚡ Performance & Scaling

### Current Capacity
- **Minimum:** 2 pods = 100-150 concurrent users
- **Maximum:** 20 pods = 1,000-1,500 concurrent users
- **Auto-scaling:** Triggers at 50% CPU or 70% memory

### Scaling Configuration
```bash
# Increase max replicas for larger events
kubectl patch hpa pooja-invitation-hpa -n pooja-invitation -p '{"spec":{"maxReplicas":50}}'

# Force immediate scaling (testing)
kubectl scale deployment pooja-invitation --replicas=5 -n pooja-invitation
```

### Performance Testing
```bash
# Generate load to test scaling
kubectl run load-test --image=busybox -n pooja-invitation --rm -it --restart=Never -- /bin/sh

# Inside the pod:
while true; do wget -q -O- http://pooja-invitation-service; sleep 0.1; done
```

## 🛠 Troubleshooting

### Common Issues

**Pods not starting:**
```bash
kubectl describe pods -n pooja-invitation
kubectl logs -l app=pooja-invitation -n pooja-invitation
```

**HPA not scaling:**
```bash
# Check metrics server
kubectl top nodes
kubectl top pods -n pooja-invitation

# If metrics-server missing:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

**Ingress not working:**
```bash
# Check ingress controller
kubectl get pods -n ingress-nginx

# Install nginx-ingress if missing
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.2/deploy/static/provider/cloud/deploy.yaml
```

### Resource Cleanup
```bash
# Delete everything
kubectl delete namespace pooja-invitation

# Or delete individually
kubectl delete -f k8s-ingress.yaml
kubectl delete -f k8s-hpa.yaml
kubectl delete -f k8s-deployment.yaml
kubectl delete -f k8s-namespace.yaml
```

## 📱 Sharing with Guests

### Direct Links
- **Primary:** http://pooja.shrinidhi.space
- **Backup:** Share your external IP if domain fails

### QR Code Generation
```bash
# Generate QR code for easy mobile sharing
echo "http://pooja.shrinidhi.space" | qrencode -t PNG -o invitation-qr.png
```

### WhatsApp/Social Sharing
```
🙏 You're invited to our Pooja!
📅 Sunday, 26 October
🔗 http://pooja.shrinidhi.space
📍 Direct Google Maps navigation included
```

## 🔒 Security (Optional)

### Enable HTTPS
```bash
# Install cert-manager
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.13.0/cert-manager.yaml

# Add TLS to ingress (edit k8s-ingress.yaml)
# spec:
#   tls:
#   - hosts:
#     - pooja.shrinidhi.space
#     secretName: pooja-tls
```

## 📈 Deployment Timeline

**Week before Pooja:**
- Deploy and test all components
- Share initial invitations
- Monitor for any issues

**Day of Pooja:**
- Monitor scaling dashboard
- Expected traffic spike when guests check location
- Auto-scaling will handle the load

**After Pooja:**
- Pods will automatically scale down
- Consider cleanup or keeping for future events

---

## Files Structure
```
invitation/
├── index.html              # Main invitation page
├── style.css               # Styling
├── Dockerfile              # Container config
├── k8s-namespace.yaml      # Kubernetes namespace
├── k8s-deployment.yaml     # Deployment + Service
├── k8s-hpa.yaml           # Auto-scaling config  
├── k8s-ingress.yaml       # Domain routing
└── k8s-README.md          # This file
```

**You're ready to deploy! 🎉**
