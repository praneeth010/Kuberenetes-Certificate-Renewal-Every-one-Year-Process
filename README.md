# Kuberenetes-Certificate-Renewal-Every-one-Year-Process
Step-by-step guide to check, renew, and manage Kubernetes certificates using kubeadm before expiry in production clusters.

#Step are DONE BY ME

CHECK THE EXPIRY OF CERTIFICATES
```bash
kubeadm certs check-expiration
```
or
```bash
kubeadm alpha certs check-expiration
```

RENEW THE CERTIFICATES
```bash
sudo kubeadm certs renew all
```

RESTART KUBERNETES COMPONENTS
```bash
sudo systemctl restart kubelet
```

UPDATE THE KUBECONFIG FILES (IF NEEDED)
```bash
sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config
```
```bash
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

# 🚀 Kubernetes Certificate Renewal (Yearly Process)

## 📌 Overview

Kubernetes certificates expire every 1 year in kubeadm-based clusters.
This guide explains how to check, renew, and verify certificates to avoid cluster downtime.

---

## 🔍 Step 1: Check Certificate Expiry

```bash
sudo kubeadm certs check-expiration
```

---

## 💾 Step 2: Backup Kubernetes Configurations

```bash
sudo cp -r /etc/kubernetes /etc/kubernetes-backup
```

---

## 🔄 Step 3: Renew All Certificates

```bash
sudo kubeadm certs renew all
```

---

## 🔁 Step 4: Restart Kubernetes Components

```bash
sudo systemctl daemon-reexec
sudo systemctl restart kubelet
```

---

## ⚙️ Step 5: Update Kubeconfig

```bash
mkdir -p $HOME/.kube
sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

## ✅ Step 6: Verify Renewal

```bash
sudo kubeadm certs check-expiration
```

Expected:

* Certificates valid for ~365 days

---

## 🚀 Step 7: Validate Cluster

```bash
kubectl get nodes
kubectl get pods -A
```

---

## 🟡 Worker Node Notes

* No manual certificate renewal required
* Kubelet handles automatic rotation

Optional:

```bash
sudo systemctl restart kubelet
```

---

## ⚠️ Important Notes

* CA certificates are valid for 10 years
* Always renew before expiry
* Backup is mandatory before renewal

---

## 🔥 Interview Tip

Kubernetes certificates are renewed using `kubeadm certs renew all`, followed by kubelet restart and cluster validation.

---

## 📌 Author

DevOps Engineer | Kubernetes | AWS | Terraform
