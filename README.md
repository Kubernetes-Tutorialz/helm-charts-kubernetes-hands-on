# Helm

## Sumario

- [Helm](#helm)
  - [Sumario](#sumario)
  - [What is Helm](#what-is-helm)
  - [A graduated project (CNCF)](#a-graduated-project-cncf)
  - [Installing Helm](#installing-helm)
    - [Before to proceed, test your application](#before-to-proceed-test-your-application)
      - [Checking the deployment:](#checking-the-deployment)
      - [Checking the POD's:](#checking-the-pods)
      - [Checking the services:](#checking-the-services)
    - [Criando nossa estrutura do Helm](#criando-nossa-estrutura-do-helm)
    - [Visualizando a estrutura criada](#visualizando-a-estrutura-criada)
    - [Hora de fazer o deploy](#hora-de-fazer-o-deploy)
  - [Contributing](#contributing)
  - [License](#license)

## What is Helm
O [Helm](https://www.cncf.io/projects/helm/) é um projeto graduado da CNCF, que seu proposito é atuar como um gerenciador de pacotes criado para facilitar a instalação de aplicações e suas dependências no Kubernetes. Podemos comparar o Helm com o `apt-get` do Debian, pois com apenas um comando você consegue instalar aplicações e suas dependencias no Kubernetes e ainda, fazer o gerenciamento de suas versões, podendo fazer o upgrade ou downgrade sem maiores problemas e rapidamente. O Helm não é somente utilizado para fazer a instalação de aplicativos de terceiros, você consegue criar charts, que são os pacotes que o Helm utiliza para a instalação e configuração do aplicativo no Kubernetes. O chart é composto por arquivos que definem como e qual deve ser o comportamento da aplicação dentro do cluster. É no chart que você define o seu `deployment`, o `service`, `ingress` e qualquer outra coisa necessária para a instalação e configuração da app desejada, e para isso, utilizamos os templates, que serão abordados mais para frente.

## A graduated project (CNCF)

Estamos falando de um projeto bem legal da CNCF, então é justo a gente abordar um pouco sobre Cloud Native que é uma iniciativa TOP de mercado, com muitos projetos legais que estão classificados em graduados, incubados e sandbox. O próprio Kubernetes é mantido pela CNFF, graduado e em constante evolução pela comunidade. Aqui na página do Helm do [GitHub](https://github.com/helm/helm) possui mais detalhes sobre a ferramenta, commits, PRs e issues que estão sendo tratadas pela comunidade.

## Installing Helm

Para se fazer a instalação do Helm no LINUX, basta seguir os passos abaixo, é bem simples, vamos optar por fazer a instalação via script. Para outros sistemas, acesse esse link aqui e faça a instalação de acordo com seu sistema utilizado.

```bash
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

### Before to proceed, test your application

Para que nao tenhamos surpresas durante o deploy usando Helm Charts, temos que testar nossa aplicacao para garantir que TUDO esteja funcional demais. Para isso, com o cluster já em execução, execute os comando abaixo para que você possa deployar o `deployment.yml` e o outro arquivo `service.yml`. Vamos usar aqui como exemplo um servidor web `NGINX` para que possamos exemplificar da melhor forma o workflow de deploy do Helm Chart.

- Deployando primeiro o arquivo de `deployment`:

`kubectl create deployment nginx03 --image=nginx`

- Criando nosso arquivo de `services`:

`kubectl create services -f nginx_helm_service.yml`

#### Checking the deployment:

```bash
# kubectl get deployments.apps 
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
grafana-labz          1/1     1            1           2d8h
nginx-example         1/1     1            1           30h
nginx02               1/1     1            1           19h
nginx03               1/1     1            1           10h
primeiro-deployment   1/1     1            1           29h
```

#### Checking the POD's:

```bash
# kubectl get pods
NAME                                   READY   STATUS    RESTARTS        AGE
busybox                                1/1     Running   28 (20m ago)    28h
busybox-configmap-file                 1/1     Running   105 (14m ago)   7d11h
grafana-labz-6db6bc44bb-xshdl          1/1     Running   0               2d8h
init-demo                              1/1     Running   0               3d21h
nginx                                  1/1     Running   2 (4d4h ago)    12d
nginx-example-747ddb8bc7-tmp8n         1/1     Running   0               30h
nginx02-7d786956cd-gh2pp               1/1     Running   0               19h
nginx03-5999959778-bt588               1/1     Running   0               10h
primeiro-deployment-66b459c7d8-jkzqh   1/1     Running   0               29h
webserver                              1/1     Running   1 (4d4h ago)    9d
```

#### Checking the services:

```bash
# kubectl get svc
NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes            ClusterIP   10.96.0.2        <none>        443/TCP        12d
nginx                 NodePort    10.100.128.83    <none>        80:30907/TCP   29h
nginx03               NodePort    10.108.254.206   <none>        80:32453/TCP   10h
nginxhelm             NodePort    10.106.167.203   <none>        80:31355/TCP   17h
primeiro-deployment   NodePort    10.102.126.167   <none>        80:32573/TCP   29h
```

Veja que eu fiz deploy dos arquivos manualmente, ou seja, tive que rodar o comando `kubectl` para subir os arquivos `YML` de nome`deployments`, `services`. A ideia do Helm comeca agora, pois ele vai entrar como um gerenciador de pacotes dentro do Kubernetes.
 
### Criando nossa estrutura do Helm

Para que possamos criar nossa estrutura do Helm Chart completa, existe um comando que faz isso de forma mais legal e automatizada.

`$ helm create nginxcharts`

### Visualizando a estrutura criada

```bash
# tree nginxcharts/
nginxcharts/
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── NOTES.txt
│   ├── serviceaccount.yaml
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

3 directories, 10 files
```

### Hora de fazer o deploy

```bash
# helm install ./nginxcharts --generate-name 
NAME: nginxcharts-1656402303
LAST DEPLOYED: Tue Jun 28 04:45:04 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services nginxcharts-1656402303)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[GPL-3.0 license](https://github.com/Kubernetes-Tutorialz/helm-charts-kubernetes-hands-on/blob/main/LICENSE)