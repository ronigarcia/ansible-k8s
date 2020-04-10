# Criação de cluster K8S com Ansible

## Objetivo 1

Esse projeto cria três instancias EC2 na AWS, instala o Kubernetes, o HELM e o Prometheus.

Playbook provisioning

## Objetivo 2

Subir uma aplicação de teste no cluster K8S com a versão 1 e depois atualizar para a versão 2 com o método canary

Playbooks:
deploy_app_v1
deploy_app_v1

## Como utilizar esse projeto

Configurando o arquivo provisioning/roles/criando-instancias/vars/main.yml conforme orientações abaixo:

Altere variável "profile" de acordo com o nome do profile aws configurado em sua máquina.

É necessário que já exista uma keypair criada na AWS para acesso as instancias, que deve ser adicionada ao arquivo de variável.

Para cirar as instancias, instalar o configurar o cluster execute o arquivo main.yml no diretório provisioning.

```python
ansible-playbook -i hosts main.yml
```

Esse playbook irá chamar as roles:
- create-instance (cria as instancias na AWS e adiciona os ips no arquivo hosts);

- install-k8s (instala docker e k8s);

- create-cluster (inicia o cluster k8s no node master);

- join-workers (adiciona os demais nodes como worker no cluster);

- install-helm (instala o helm no cluster);

- install-monit-tools (Instala os componentes de monitoramento do cluster).

## Subindo aplicações de teste

Copie o arquivo hosts do diretório provisioning para os diretorios deploy_app_v1 e deploy_app_v2

Por padrão serão iniciadas 10 replicas no cluster, para alterar vá no arquivo de variável deploy_app_v1/roles/deploy-app/vars/main.yml.

Execute o arquivo main.yml no diretório deploy_app_v1

```python
ansible-playbook -i hosts main.yml
```

Para validar que a aplicação esta funcionando, acesse por um navegador o IP do node master na porta 32222.



Para atualizar a versão do app execute o arquivo main.yml no diretório deploy_app_v2

```python
ansible-playbook -i hosts main.yml
```

Para validar que a aplicação esta funcionando, acesse por um navegador o IP do node master na porta 32222.

Agora estarão 10  replicas da versão 2 e 1 replica da versão 1.
Essas configurações estão no arquivo de variável deploy_app_v2/roles/deploy-app/vars/main.yml
