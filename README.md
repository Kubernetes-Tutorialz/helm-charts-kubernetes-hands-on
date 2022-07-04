# Helm

# Summary

## Helm
O Helm é um projeto graduado da CNCF que atua como um gerenciador de pacotes criado para facilitar a instalação de aplicações e suas dependências no Kubernetes. Podemos comparar o Helm com o `apt-get` do Debian, pois com apenas um comando você consegue instalar aplicações e suas dependencias no Kubernetes e ainda, fazer o gerenciamento de suas versões, podendo fazer o upgrade ou downgrade sem maiores problemas e rapidamente. O Helm não é somente utilizado para fazer a instalação de aplicativos de terceiros, você consegue criar charts, que são os pacotes que o Helm utiliza para a instalação e configuração do aplicativo no Kubernetes. O chart é composto por arquivos que definem como e qual deve ser o comportamento da aplicação dentro do cluster. É no chart que você define o seu `deployment`, o `service`, `ingress` e qualquer outra coisa necessária para a instalação e configuração da app desejada, e para isso, utilizamos os templates, que serão abordados mais para frente.
 

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