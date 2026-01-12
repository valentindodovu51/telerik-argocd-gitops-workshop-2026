# Argo CD GitOps Cheat Sheet

## 📋 Common kubectl Commands

### Check Application Pods
```bash
kubectl get pods -n demo
kubectl describe pod <pod-name> -n demo
kubectl logs <pod-name> -n demo
```

### Check Deployment Status
```bash
kubectl get deployment demo-app -n demo
kubectl describe deployment demo-app -n demo
```

### Check Service
```bash
kubectl get service demo-service -n demo
kubectl describe service demo-service -n demo
```

### Check All Resources in Namespace
```bash
kubectl get all -n demo
```

### Watch Resources in Real-Time
```bash
kubectl get pods -n demo -w
```

## 🔄 Argo CD Application States

### Sync Status
- **Synced** – Cluster matches Git (desired state)
- **OutOfSync** – Cluster differs from Git (drift detected)
- **Unknown** – Argo CD cannot determine sync status

### Health Status
- **Healthy** – Application is running correctly
- **Progressing** – Application is deploying/updating
- **Degraded** – Application has issues (pods failing, etc.)
- **Suspended** – Application is paused
- **Missing** – Resources are missing from cluster
- **Unknown** – Health cannot be determined

### Important Note
**Synced ≠ Healthy**
- An application can be Synced (matches Git) but Degraded (not working)
- Always check both sync status AND health status

## 🛠️ Troubleshooting Tips

### Application Won't Sync
1. Check Argo CD Application logs:
   ```bash
   kubectl logs -n argocd deployment/argocd-application-controller
   ```

2. Check repository access:
   ```bash
   kubectl get applications -n argocd
   kubectl describe application demo-app -n argocd
   ```

3. Manually trigger sync:
   ```bash
   # Using Argo CD CLI (if installed)
   argocd app sync demo-app
   ```

### Pods Stuck in ImagePullBackOff
- Check image name and tag in `app/deployment.yaml`
- Verify image exists in registry
- Check pod events: `kubectl describe pod <pod-name> -n demo`

### Pods in CrashLoopBackOff
- Check logs: `kubectl logs <pod-name> -n demo`
- Check previous logs: `kubectl logs <pod-name> -n demo --previous`
- Verify container configuration

### Application Shows OutOfSync
- Someone made manual changes with kubectl
- Wait for self-heal to fix it automatically
- Or check what's different in Argo CD UI

### Self-Heal Not Working
1. Check if automated sync is enabled in `argocd/application.yaml`:
   ```yaml
   syncPolicy:
     automated:
       selfHeal: true
   ```

2. Check Argo CD sync settings:
   ```bash
   kubectl get application demo-app -n argocd -o yaml
   ```

## ✨ GitOps Golden Rules

### Rule 1: Never kubectl apply Application Resources
❌ **Wrong:**
```bash
kubectl apply -f app/deployment.yaml
```

✅ **Right:**
```bash
git add app/deployment.yaml
git commit -m "Update deployment"
git push
# Let Argo CD sync it
```

### Rule 2: Git is the Single Source of Truth
- All changes must go through Git
- Manual cluster changes will be reverted
- If it's not in Git, it doesn't exist

### Rule 3: Declarative > Imperative
❌ **Wrong:**
```bash
kubectl scale deployment demo-app --replicas=3
kubectl set image deployment/demo-app nginx=nginx:1.26
```

✅ **Right:**
Edit `app/deployment.yaml`, commit, and push.

### Rule 4: Rollback = Git Revert
❌ **Wrong:**
```bash
kubectl rollout undo deployment/demo-app
```

✅ **Right:**
```bash
git revert HEAD
git push
```

### Rule 5: Observe and Learn
- Watch Argo CD UI during changes
- Understand sync vs health status
- Learn from drift detection

## 🚀 Quick Reference

### Deploy a Change
```bash
# 1. Edit files in app/
# 2. Commit and push
git add .
git commit -m "Describe your change"
git push
# 3. Watch Argo CD sync automatically
```

### Rollback a Change
```bash
git revert HEAD
git push
```

### Check Application Status
- **UI:** https://localhost:8080 (after port-forward)
- **CLI:** Check pod status in namespace

### Access Argo CD UI
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
# Open https://localhost:8080
# Username: admin
# Password: kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

---

**Remember:** GitOps is about discipline. Resist the urge to use kubectl for changes. Embrace the Git workflow!
