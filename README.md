- [DevOps](#devops)
- [Docker](#docker)
- [Rancher](#rancher)
  - [K3S.IO](#k3sio)
  - [RANCHER.OS](#rancheros)
  - [LONGORN](#longorn)
- [Kubernets](#kubernets)
  - [Kubernets master](#kubernets-master)
  - [Workers](#workers)
  - [Componentes](#componentes)
- [DevOps](#devops-1)
  - [SSH’s](#sshs)
  - [Instalação do docker via comando rancher](#instalação-do-docker-via-comando-rancher)
  - [Instalação do docker compose na máquina master](#instalação-do-docker-compose-na-máquina-master)
  - [Github da aula](#github-da-aula)
  - [Rancher single node](#rancher-single-node)
  - [Kubernets](#kubernets-1)
    - [KUBECTL](#kubectl)
  - [DNS - TRAEFIK](#dns---traefik)
- [SEGUI AS NOTAS DO PROFESSOR](#segui-as-notas-do-professor)

# DevOps

Link do curso: [https://www.udemy.com/course/devops-mao-na-massa-docker-kubernetes-rancher/learn/lecture/19506988#content](https://www.udemy.com/course/devops-mao-na-massa-docker-kubernetes-rancher/learn/lecture/19506988#content)

# Docker

Container são feitos para serem descartados. Dados importantes devem ser guardados fora dos containers, pois se um container morrer ele pode ser recriado a reconectado ao volume sem problemas.

Containers virtualizam o sistema operacional

# Rancher

[https://rancher.com/](https://rancher.com/)

Gerenciamento e orquestração de containers. Gerenciador de kubernets.

Tem a intenção de facilitar a implantação e manutenção do Kubernets, facilitando a comunicação entre kubernets, monitoramento, backup e etc…

**Serviços**

- Rede
- Armazenamento
- Volumes
- Balanceador de carga
- DNS

Produtos do Rancher

## K3S.IO

Kubernets muito leve, utilizado em IOT ou ambientes que não tenham muito poder de processamento.

## RANCHER.OS

[https://rancher.com/docs/os/v1.x/en/](https://rancher.com/docs/os/v1.x/en/)

Sistema operacional próprio para rodar containers. Super leve, tem somente o necessário para rodar os containers

## LONGORN

Utilizado para trabalhar com volumes de containers no cluster

# Kubernets

Desenvolvido pelo Google a partir do google borg

Ferramenta de gerenciamento e orquestração de containers.

Consiste em pelo menos um master e multiplos nodes de computação

O master é responsável pelo API, agendamento dos deployments e gerenciamento total dos clusters.

> Um cluster suporta até 5000 máquinas
> 

## Kubernets master

Ele possui:

- API server
- Scheduler
- Controller
- etcd

Através disso ele faz a orquestração dos container nos nodes workers

## Workers

Cada nó do cluster roda:

- Container runtime, Docker ou Rocket
- Componentes adicionais para logs, monitoramento, service discovery e add-ons opcionais.

## Componentes

**Pods** - Containers que trabalham juntos

**Services** - Pods que trabalham juntos

**Deployments** - Provê uma única declaração para pods e ReplicaSets

**Labels** - Usado para organização dos serviços

**Daemonsets** - Rodar sempre um ou mais pods por nó

**Secrets** - Salvar os dados sensiveis como senhas

**ConfigMaps** - Arquivo de configuração que as aplicações irão utilizar

**CronJobs** - Executar tarefas agendadas, uma ou repetidas vezes

# DevOps

Prática @-@

Para isso foi utilizado 4 maquinas delicinhas da google cloud, um dominio para lascarmos um https bonito e agora bora arrasar…

> Cnfigurar a VPC de sua cloud é bem importante, então cuidado para não vacilar, senão sua máquina não vai se comunicar direito :#
> 

**Máquinas:**

2 Vcpus

6 GB ram

30 GB de armazenamento

## SSH’s

```bash
ssh -i id_rsa 35.199.87.142 #master
ssh -i id_rsa 35.215.227.139 #worker 1
ssh -i id_rsa 35.215.195.82 #worker 2
ssh -i id_rsa 35.215.249.65 #worker 3
```

## Instalação do docker via comando rancher

Ao iniciar as máquinas rode o comando abaixo:

[https://rancher.com/docs/rancher/v2.5/en/installation/requirements/installing-docker/](https://rancher.com/docs/rancher/v2.5/en/installation/requirements/installing-docker/)

```bash
curl https://releases.rancher.com/install-docker/20.10.sh | sh
sudo usermod -aG docker user
```

## Instalação do docker compose na máquina master

Para instalar o docker compose na máquina master, utilize o comando abaixo:

```bash
apt install docker-compose
```

## Github da aula

```bash
git clone https://github.com/jonathanbaraldi/devops
```

## Rancher single node

Rodando o Rancher single node em um container

[https://rancher.com/quick-start](https://rancher.com/quick-start)

[https://rancher.com/docs/rancher/v2.5/en/installation/other-installation-methods/single-node-docker/](https://rancher.com/docs/rancher/v2.5/en/installation/other-installation-methods/single-node-docker/)

```bash
sudo docker run --privileged -d --restart=unless-stopped -v /opt/rancher:/var/lib/rancher -p 80:80 -p 443:443 rancher/rancher
```

**Senha criada**: 123456789123456789

## Kubernets

Criando os nodes em kubernets para rodar nossas aplicações

k8s1

```bash
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.4.3 --server https://rancher.williansoncini.com --token sjf4kn28lzfnwsvd95mrbkp4kgh6hk4cxlwwqm2l7srdc4nb8nqh4h --ca-checksum 68b978e3c9de29961c14609da70a5ed9ab855f88fdfcc2411d56088c87a8ed1d --node-name worker- --etcd --controlplane --worker
```

### KUBECTL

**Linha de comando do kubernets.**

Instalação padrão de acordo com a documentação

[https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

Após isso você deve **copiar** o arquivo de configuração KubeConfig através da plataforma do Rancher

E colar ele dentro do caminho `~/.kube/config`

```bash
mkdir -p ~/.kube # Para caso de algum erro de pasta
vi ~/.kube/config
```

**Exemplo de comandos**

Pegar nós

```bash
kubectl get nodes
#k8s1   Ready    controlplane,etcd,worker   78m   v1.23.6
#k8s2   Ready    controlplane,etcd,worker   69m   v1.23.6
#k8s3   Ready    controlplane,etcd,worker   64m   v1.23.6
```

Pegar os pods dos nós

```bash
get pods -n lube-system
```

## DNS - TRAEFIK

Em vez de se utilizar um Nginx por exemplo, agora utilizamos o Traefik, que permite fazer o proxy para os containers.

Para isso foi criado um novo subdominio da seguinte forma

*.rancher.domain.com → Apontando para os ip’s dos clusters kubernets.

**Configurações necessárias**

Configurando o `traefik-ingress-controller`

Primeiro vamos configurar o acesso do traefik (RBAC - Role basic access control)

Tive que fazer o arquivo `traefik-RBAC.yaml` (Pode ser qualquer nome)

```bash
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: traefik-ingress-controller
rules:
  - apiGroups:
      - ""
    resources:
      - services
      - endpoints
      - secrets
    verbs:
      - get
      - list
      - watch
  - apiGroups:
      - extensions
    resources:
      - ingresses
    verbs:
      - get
      - list
      - watch
  - apiGroups:
    - extensions
    resources:
    - ingresses/status
    verbs:
    - update
---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: traefik-ingress-controller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: traefik-ingress-controller
subjects:
- kind: ServiceAccount
  name: traefik-ingress-controller
  namespace: kube-system
```

E rodar o comando abaixo:

```bash
kubectl apply -f traefik-RBAC.yaml
#clusterrolebinding.rbac.authorization.k8s.io/traefik-ingress-controller created
```

Criando o `serviço` para o traefik-ingress-controller

- Aqui também é setado para ficar sempre um pod rodando em cada cluster

```bash
kubectl apply -f https://raw.githubusercontent.com/containous/traefik/v1.7/examples/k8s/traefik-ds.yaml
```

Para testar você pode rodar o comando abaixo:

```bash
kubectl --namespace=kube-system get pods
```

Agora é hora de configurar o DNS

no repositório do curso existe a pasta do treinamento do kubernets com os exercícios, rode o comando abaixo:

```bash
cd treinamento-kubernetes/exercicios/
kubectl apply -f ui.yml
```

# SEGUI AS NOTAS DO PROFESSOR

> Foi mais vantajoso seguir as notas do professor. Mas posteriormente devo voltar e anotar aqui.
> 
> Ainda preciso entender muita coisa disso :)

Notas do professor super completas

https://github.com/jonathanbaraldi/devops
