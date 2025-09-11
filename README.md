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

# Projeto GitOps - Online Boutique

## Objetivo

Executar um conjunto de microserviços (**Online Boutique**) em Kubernetes local usando Rancher Desktop, controlado por GitOps com ArgoCD, a partir de um repositório público no GitHub.

---

## Passo a Passo

### 1. Instalar Rancher Desktop

- Baixe e instale o Rancher Desktop: [https://rancherdesktop.io/](https://rancherdesktop.io/)  
- Durante a instalação, selecione **Kubernetes** como runtime.

---

### 2. Instalar Kubernetes e configurar kubectl

```bash
# Verificar nodes do cluster
kubectl get nodes
````

### 3. Instalando o Argocd no cluster
````bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
````

### 4. Acessando o ArgoCD localmente
````bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
````
Acesse o navegador: 
````bash
https://localhost:8080/
````
Usuário: admin (por padrão do Argo)
senha: Voce precisará desse comando
````bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
````
e ele retornará a senha pra você

### 9. Criar o App no ArgoCD 🚀

Siga os passos abaixo na interface web do ArgoCD:

Clique em **New App**
Preencha as configurações do aplicativo:

   **Application Name:** `online-boutique`  
   **Project:** `default`  
   **Repository URL:** *URL do seu repositório público*  
   **Path:** `k8s`  
   **Cluster:** `https://kubernetes.default.svc`  
   **Namespace:** `default`  

Clique em **Create**

Após isso, espere todos os pod's serem processados e healthy

### 10. Acessar o frontend do app
````bash
kubectl port-forward svc/frontend -n default 8081:80
````
abrir o navegador no localhost:
````bash
http://localhost:8081/
````
