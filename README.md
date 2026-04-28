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

## 🟨 Recommended Validation on Worker Nodes

After renewing certificates on the control-plane node, worker nodes usually do not require manual certificate renewal because kubelet automatically rotates its client certificates.

However, it is recommended to validate kubelet status and worker node health.

---

# Step 8: Check kubelet Status

Login to each worker node and verify kubelet is running.

```bash
sudo systemctl status kubelet
```

Expected Output:

```bash
active (running)
```

---

# Step 9: Restart kubelet (Optional but Recommended)

Restart kubelet service after master certificate renewal.

```bash
sudo systemctl restart kubelet
```

---

# Step 10: Verify Worker Node Certificate Expiry

Check kubelet client certificate expiry date.

```bash
sudo openssl x509 -in /var/lib/kubelet/pki/kubelet-client-current.pem -text -noout | grep "Not After"
```

Example Output:

```bash
Not After : Apr 28 2027 GMT
```

---

# 🟥 If Worker Node Shows NotReady

Check cluster node status from the master node.

```bash
kubectl get nodes
```

Example Problem:

```bash
worker1   NotReady
```

If the worker node is in `NotReady` state, follow the steps below.

---

# 🔥 Fix Worker Node Certificate

## Step 1: Remove Old kubelet Certificates

Run on the worker node:

```bash
sudo rm -f /var/lib/kubelet/pki/kubelet-client*
```

---

## Step 2: Restart kubelet

```bash
sudo systemctl restart kubelet
```

---

## Step 3: Approve CSR from Master Node

Check pending certificate signing requests from the control-plane node.

```bash
kubectl get csr
```

Approve the pending CSR:

```bash
kubectl certificate approve <csr-name>
```

Example:

```bash
kubectl certificate approve csr-abcd1
```

---

# 🟩 Final Validation

Run the following commands on the master node:

```bash
kubectl get nodes
kubectl get pods -A
kubectl get csr
```

Expected Result:

```bash
All nodes should be in Ready state
```

---

# 📌 Important Understanding

| Component                               | Renewal Needed |
| --------------------------------------- | -------------- |
| Master certificates                     | YES            |
| Worker kubelet certificate              | Auto rotation  |
| Copy certificates from master to worker | NO             |
| Restart kubelet on worker               | Recommended    |
| `kubeadm certs renew all` on worker     | NO             |

---

# 🔥 Interview Tip

In kubeadm-based Kubernetes clusters, worker node kubelet certificates are automatically rotated. If rotation fails, delete old kubelet client certificates, restart kubelet, approve the CSR from the master node, and verify the node returns to Ready state.


## 🔥 Interview Tip

Kubernetes certificates are renewed using `kubeadm certs renew all`, followed by kubelet restart and cluster validation.

---

## 📌 Author

DevOps Engineer | Kubernetes | AWS | Terraform
