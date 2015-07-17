# Tutorial de JBoss BPM Suite 6.1

## Instalação das ferramentas

```bash
tar xfz apache-maven-3.3.3-bin.tar.gz
unzip -q jboss-eap-6.4.0.zip
unzip -oq jboss-bpmsuite-6.1.0.GA-deployable-eap6.x.zip 
```

## Configuração do ambiente

```bash
cat << EOF > ambiente.sh
export M2_HOME=$PWD/apache-maven-3.3.3
export PATH=\$M2_HOME/bin:\$PATH
export JBOSS_HOME=$PWD/jboss-eap-6.4
export PATH=\$JBOSS_HOME/bin:\$PATH
EOF
source ambiente.sh
```

## Configuração do Maven

```bash
mv ~/.m2 ~/.m2.$(date "+%Y%m%d-%H%M%S")
mkdir ~/.m2
git clone https://github.com/jboss-developer/jboss-brms-quickstarts.git
cd jboss-brms-quickstarts/
git tag -l
git checkout BxMS-6.1.0.GA
cp settings.xml ~/.m2
```

## Configuração do JBoss

### Adição de usuário para acesso ao Business Central

```
add-user.sh -a -u 'quickstartUser' -p 'quickstartPwd1!' -ro 'admin,analyst'
```

### Ajustes no standalone.xml
```bash
cd ..
d=standalone/configuration
cp jboss-eap-6.4/$d/standalone.xml{,.original}
patch jboss-eap-6.4/$d/standalone.xml jboss-eap-6.4.files/$d/standalone.xml.patch
```

## Execução do JBoss

```bash
standalone.sh &
```

## Execução do helloworld-bpmsuite

### URL de acesso a aplicação no GitHub

https://github.com/jboss-developer/jboss-brms-quickstarts/tree/BxMS-6.1.0.GA/helloworld-bpmsuite

### Importação do repositório BRMS

1. Acessar http://localhost:8080/business-central
1. Usuário/senha: quickstartUser/quickstartPwd1!
1. Selecionar o menu ``Authoring -> Administration``
1. Selecionar o submenu ``Repositories -> Clone Repository``
1. Na tela popup aberta, informar:
    1. Repository Name: ``jboss-brms-repository``
    1. Organizational Unit: ``example``
    1. Git URL: ``https://github.com/jboss-developer/jboss-brms-repository.git``
1. Clicar no botão ``Clone``

### Implantação do processo

1. Selecionar o menu ``Authoring -> Project Authoring``
1. Escolher as seguintes opções em ``Project Explorer``: ``example/jboss-bpms-repository/bpms-project``
1. Clicar no botão ``Open Project Editor``
1. Clicar no botão ``Build`` e selecionar ``Build & Deploy``

### Execução dos testes da aplicação

```bash
cd jboss-brms-quickstarts/helloworld-bpmsuite
mvn clean test -Penable-test,bpms
```

## Limpeza (para reinicialização do tutorial)

```bash
rm -rf jboss-brms-quickstarts
rm ambiente.sh
rm -rf apache-maven-3.3.3 jboss-eap-6.4
```
