<h1 align=center>Container - Ansible</h1>

## indice

- [Requisitos](#requisitos)
- [Criando o container](#criando-o-container)
- [Processo de instalação e configuração](#processo-de-instalação-e-configuração)
- [Instalando Ansible](#instalando-ansible)
- [Configurações de ssh](#configurações-de-ssh)
- [Adicionando hosts](#adicionando-hosts)
- [Testando](#testando)
- [Contribuindo](#contribuindo)

<p>Container criado no intuito de automatizar/facilitar a vida, quando precisamos ter que acessar mais de 150 VMs pra obter uma informação não monitorada!
</p>

## Requisitos
<p>Desejável que tenha conhecimento básico de Ansible.</p>

## Criando o container

<b>docker run -dit --name ansible_stk -h ansiblestk-pc -v /path/playbooks/:/playbooks ubuntu:20.04</b>

<ul>
<li>name : ansible_stk</li>
<li>hostname : ansiblestk-pc</li>
<li>volume : /dockers:/playbooks</li>
<li>Baseado no : Ubuntu 20.04</li>
<li>nenhuma porta mapeada a principio</li>
</ul>
<hr>

## Processo de instalação e configuração

    #update de repositórios
    apt update

    #instalação de pkgs importantes ao meu ver :
    apt install nmap openssh-server net-tools iputils-ping curl wget vim git sshpass -y

<b>Demais pkgs serão instalados se for verificado a necessidade ou dependência.</b>

## Instalando Ansible

    apt update
    apt install software-properties-common
    add-apt-repository --yes --update ppa:ansible/ansible
    apt install ansible

## Configurações de ssh :

    /etc/ssh/ssh_config

<p>Removido o comentário da linha : <br>
    
    StrictHostKeyChecking ask
e alterado o valor de "ask" para "no".
<br>Adicionado a linha : <br> 
    
    UserKnownHostsFile /dev/null
E por fim restartado o service de ssh.<br> 
Assim evitando a solicitação de adicionar o host ao know_host.</p>

## Adicionando hosts
<p>Para que o Ansible possa se comunicar com as maquinas, é necessário adicionar ao hosts do Ansible, que fica em : <b>/etc/ansible/hosts</b>. <br>
Siga o próprio exemplo do documento : <br>

    [GROUP]
    192.168.0.3
    192.168.0.4
 


<br>
É através desse hosts que o Ansible vai poder se comunicar com as maquinas. 
<br>
Organize seus hosts, adote um padrão, segue o exemplo :

    [NOME_DO_GRUPO]
    HOSTNAME      ansible_host=192.168.0.2
    HOSTNAME      ansible_host=192.168.0.3
    HOSTNAME      ansible_host=192.168.0.4

</p>

## Testando :

    ansible GROUP -m ping --ask-pass

<p>GROUP é meu grupo para as maquinas:
<ul>
<li>maquina_um</li>
<li>maquina_dois</li>
</ul>
<p>Foi utilizado o modulo ping, junto de --ask-pass considerando que você não tenha criado uma chave para usar no authorized_keys, então será solicitada a senha de root, que é o usuário padrão.</p>

<b>Caso queria usar comados de shell use : -m shell -a 'comando'</b>
<br>
<b>Dicas : Caso você queria usar outro usuário que não seja root, pode passar na cli como : -u=nome_do_usuario, ou adicionar no host, como : 

    [GROUP:vars]
    ansible_user=nome_do_usuario
</ul>
Dessa forma ao rodar o comando : ansible GROUP -m ping --ask-pass , o ansible saberá que esta conectando com outro usuário que não seja o root!</b>
<hr>
<br><b>Obs.: Não irei disponibilizar a imagem no hub docker, por ter informações confidenciais, por isso o passo a passo de como criar e configurar!</b>

## Contribuindo

<p>Qualquer contribuição que você fizer será bem vinda, analisada e se viável, atualizada no projeto!
<ul>
<li>1.Faça um fork do projeto</li>
<li>2.Clone o seu fork para sua maquina(git clone url-do-projeto.git)</li>
<li>3.Crie um branch para sua modificação/atualização(git checkout -b seu-branch)</li>
<li>4.Adicione suas alterações ao git(git add .)</li>
<li>5.Confirme suas alterações(git commit -m "texto descritivo do que fez")</li>
<li>6.Suba seu branch(git push origin seu-branch)</li>
<li>7.Faça uma solicitação de merge request para o branch : <b>updates</b></li>
</ul>
</p>