# Automatik
Para iniciar seus projetos de Kubernetes + Docker, precisaremos de instalar algumas ferramentas e dependências

# Passos para Instalar Docker
# 1. Atualize a lista de pacotes e instale os pacotes necessários:
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
```
# 2. Adicione a chave GPG oficial do Docker:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

# 3. Adicione o repositório Docker:

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

# 4. Instale o Docker:

```bash
sudo apt-get update
sudo apt-get install -y docker-ce
```

# 5. Verifique se o Docker está instalado corretamente:

```bash
sudo systemctl status docker
```

# 6. Adicione seu usuário ao grupo Docker (opcional, para evitar usar sudo com Docker):

```bash
sudo usermod -aG docker $USER
```

# Passos para Instalar Kubernetes

# 1. Adicione a chave GPG do Kubernetes:

```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

# 2. Adicione o repositório Kubernetes:

```bash
sudo bash -c 'cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
```

# 3. Atualize a lista de pacotes e instale kubeadm, kubectl e kubelet:

```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

# Configuração do Cluster Kubernetes

# 1. Desative o swap (necessário para Kubernetes funcionar corretamente):

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

# 2. Inicialize o cluster Kubernetes no nó mestre (substitua <your_cidr> pelo CIDR de rede do seu cluster):

```bash
sudo kubeadm init --pod-network-cidr=<your_cidr>
```

# 3. Configure o kubectl para o usuário não root:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#4. Implante um plugin de rede para os pods (exemplo com Calico):

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

# Adicionando Nós ao Cluster

Para adicionar nós de trabalho ao cluster, você precisará do comando kubeadm join que foi gerado no final da execução do kubeadm init. Este comando incluirá o token e a hash do nó mestre. Execute este comando em cada nó de trabalho que você deseja adicionar.

# Passos Finais

# 1. Verifique o status dos nós do Kubernetes:

```bash
kubectl get nodes
```

# 2. Verifique o status dos pods no namespace kube-system:

```bash
kubectl get pods -n kube-system
```

Com esses comandos, você deve ter um cluster Kubernetes funcional rodando no Docker em um servidor Ubuntu 20.04. Certifique-se de ajustar as configurações de rede e outras configurações de acordo com as necessidades específicas do seu ambiente.




