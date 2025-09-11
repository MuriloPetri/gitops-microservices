# 🚀 Projeto GitOps com ArgoCD: Deploy da Online Boutique no Kubernetes

## ☁️ ARQUITETURA DO PROJETO
- **Cluster Kubernetes local** gerenciado pelo Rancher Desktop.  
- **ArgoCD** como ferramenta de GitOps para sincronizar manifests do GitHub.  
- **Online Boutique** (microservices-demo da Google) implantada via YAML.  
- **kubectl port-forward** para expor o frontend da aplicação localmente.  

---

## 🛠️ COMPONENTES
🔹 **Rancher Desktop** — fornece o cluster Kubernetes local.  
🔹 **kubectl** — CLI para interação com o Kubernetes.  
🔹 **ArgoCD** — ferramenta GitOps para gerenciar os deploys.  
🔹 **GitHub** — repositório contendo os manifests YAML.  
🔹 **Online Boutique** — aplicação de microsserviços que simula uma loja virtual.  

---

## 🚀 ETAPAS DE IMPLEMENTAÇÃO

### 1. Preparar o Repositório no GitHub
1. Faça **fork** do repositório original:  
   👉 [GoogleCloudPlatform/microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo)  
2. Crie um repositório público no seu GitHub (ex.: `gitops-microservices`).  
3. Copie o arquivo `release/kubernetes-manifests.yaml` para dentro da estrutura:  
gitops-microservices/
└── k8s/
└── online-boutique.yaml

sql
Copiar código
4. Faça commit e push:  
```bash
git add .
git commit -m "Adiciona manifests da Online Boutique"
git push origin main
2. Instalar o ArgoCD
Criar namespace:

bash
Copiar código
kubectl create namespace argocd
Instalar via manifesto oficial:

bash
Copiar código
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Verificar pods:

bash
Copiar código
kubectl get pods -n argocd
3. Acessar o ArgoCD
Fazer port-forward:

bash
Copiar código
kubectl -n argocd port-forward svc/argocd-server 8080:443
Acessar no navegador:
👉 https://localhost:8080

Usuário: admin

Recuperar senha inicial:

bash
Copiar código
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
4. Criar a Aplicação no ArgoCD
Na interface do ArgoCD, clique em New Application.

Configure:

Application Name: online-boutique

Repository URL: seu repo GitHub

Path: k8s

Cluster URL: https://kubernetes.default.svc

Namespace: online-boutique

Clique em Create e depois em Sync.

5. Verificar Deploy
Listar recursos:

bash
Copiar código
kubectl get all -n online-boutique
Conferir pods:

bash
Copiar código
kubectl get pods -n online-boutique
6. Acessar o Frontend
Como o serviço é ClusterIP, faça port-forward:

bash
Copiar código
kubectl -n online-boutique port-forward svc/frontend 8080:80
Acesse no navegador:
👉 http://localhost:8080

✅ RESULTADOS FINAIS
Repositório Git público com os manifests YAML.

ArgoCD instalado e rodando no cluster.

Aplicação sincronizada e pods Running.

Frontend acessível via kubectl port-forward.

⚡ CUSTOMIZAÇÃO (Opcional)
Edite o online-boutique.yaml no seu repo.

Exemplo: aumentar número de réplicas de um Deployment:

yaml
Copiar código
spec:
  replicas: 3
Faça commit e push.

O ArgoCD aplicará automaticamente no cluster.


