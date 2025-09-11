# 🚀 Projeto GitOps com Kubernetes e ArgoCD

Este projeto demonstra a implementação de um ambiente de **microsserviços** utilizando práticas de **GitOps**.  
O objetivo é implantar a aplicação **Online Boutique** em um **cluster Kubernetes local**, gerenciado pelo **Rancher Desktop**, com deploy contínuo controlado pelo **ArgoCD**.  

O **Git** é utilizado como **fonte única da verdade** para a infraestrutura e a aplicação, garantindo que as implantações sejam **auditáveis, previsíveis e versionadas**.  

---

## ☁️ Arquitetura e Tecnologias
- 🖥️ **Orquestração de Containers:** Kubernetes (via Rancher Desktop)  
- ⚙️ **Ferramenta GitOps:** ArgoCD  
- 🛍️ **Aplicação:** Online Boutique 
- 💻 **Ambiente Local:** Rancher Desktop com Docker  

---

## 🛠️ Pré-requisitos
Antes de iniciar, garanta que você tenha os seguintes softwares instalados e configurados:

- Rancher Desktop com Kubernetes habilitado  
- `kubectl` configurado e funcional (`kubectl get nodes`)  
- Git instalado localmente  
- Conta no GitHub  
- Docker funcionando localmente (gerenciado pelo Rancher Desktop)  

---

## 🚀 Passo a Passo

### 1️⃣ Preparar o Repositório no GitHub
1. **Fork do Repositório Original**  
   👉 [GoogleCloudPlatform/microservices-demo](https://github.com/GoogleCloudPlatform/microservices-demo)  

2. **Crie seu Repositório GitOps**  
   - Crie um novo repositório público no GitHub (ex.: `gitops-microservices`)  

3. **Estruture os Manifestos**  
   - Copie o arquivo `release/kubernetes-manifests.yaml` para dentro da seguinte estrutura:  

gitops-microservices/
└── k8s/
└── online-boutique.yaml

sql
Copiar código

4. **Commit e Push**  
```bash
git add .
git commit -m "Adiciona manifests da Online Boutique"
git push origin main
2️⃣ Instalar o ArgoCD
Criar namespace:

bash
Copiar código
kubectl create namespace argocd
Instalar o ArgoCD no cluster:

bash
Copiar código
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
Verificar os pods:

bash
Copiar código
kubectl get pods -n argocd
3️⃣ Acessar a Interface do ArgoCD
Fazer port-forward:

bash
Copiar código
kubectl -n argocd port-forward svc/argocd-server 8080:443
Acessar no navegador:
👉 https://localhost:8080

Credenciais de login:

Usuário: admin

Senha inicial:

bash
Copiar código
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
4️⃣ Criar e Sincronizar a Aplicação no ArgoCD
Na interface do ArgoCD, clique em New Application

Configure os parâmetros:

Application Name: online-boutique

Project Name: default

Repository URL: URL do seu repositório GitHub

Path: k8s

Cluster URL: https://kubernetes.default.svc

Namespace: online-boutique

Clique em Create e depois em Sync para aplicar os manifests.

5️⃣ Acessar a Aplicação
Verifique os recursos implantados:

bash
Copiar código
kubectl get all -n online-boutique
Confirme se os pods estão rodando:

bash
Copiar código
kubectl get pods -n online-boutique
Exponha o frontend (serviço ClusterIP) com port-forward:

bash
Copiar código
kubectl -n online-boutique port-forward svc/frontend 7070:80
Acesse no navegador:
👉 http://localhost:7070

✅ Entregas Esperadas
📂 Repositório Git público com a estrutura de manifests YAML

⚙️ ArgoCD instalado corretamente no cluster

🔄 Aplicação criada no ArgoCD e vinculada ao repositório Git

🚀 Aplicação sincronizada e pods em estado Running

🌐 Frontend acessível via kubectl port-forward
