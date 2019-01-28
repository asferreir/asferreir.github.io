---
layout: post
title:  "Ansible For Windows - Testando"
date:   2019-01-28 17:20
categories: [Ansible, Windows]
---

Agora começa a brincadeira.
Para manter tudo organizado vou criar uma pasta onde ficarão todos os arquivos, playbooks, varieraveis e inventory.
{% highlight ruby %}
[vagrant@ansible lab.local]$ mkdir lab.local
[vagrant@ansible lab.local]$ cd lab.local/
{% endhighlight %}

Para crio o arquivo de inventario, onde vai ficar armazenado todos os endereços que desejo gerenciar com o Ansible.
{% highlight ruby %}
[vagrant@ansible lab.local]$ vi inventory.yml
{% endhighlight %}

Como atualmente so tenho uma maquina, apenas insiro o seu IP e atribuo o grupo de web para ela.

{% highlight ruby %}
---
[web]
192.168.57.4
{% endhighlight %}

Crio a pasta group_vars, onde serão declaradas todas as variaveis usadas no ambiente.
{% highlight ruby %}
[vagrant@ansible lab.local]$ mkdir group_vars
{% endhighlight %}


E dentro dela, crio o arquivo web.yml onde irei colocar as varieris para o grupo web declado no inventory.yml
{% highlight ruby %}
[vagrant@ansible lab.local]$ vi group_vars/web.yml

ansible_user: vagrant #Usuario que o Ansible ira utilizar para conexão.
ansible_password: vagrant #Senha do usuario
ansible_port: 5985 #Porta utilizada para conexao. 
ansible_connection: winrm #protocolo utilizado para se comunicar com computadores windows
ansible_winrm_cert_validation: ignore #Ignorar a validação de certificado.
{% endhighlight %}

Caso diferente de mim, voce nao esteja usando a imagem pronta no vagrant, será necessario habilitar no windows para que ele possa receber comandos remoto atraves do powershell. Para isso é so salvar o script [ConfigureRemotingForAnsible.ps1][pws] e executar ele no powershell da maquina windows, onde ele ira configurar o Windows Remote Management (WinRM). E também execute essas 2 linhas.
{% highlight ruby %}
Set-Item -Path WSMan:\localhost\Service\Auth\Basic -Value $true
 winrm set winrm/config/service '@{AllowUnencrypted="true"}'
 {% endhighlight %}

Após esse ponto ja estamos pronto para executar o primeiro comando no Ansible.
{% highlight ruby %}
[vagrant@ansible lab.local]$ ansible web -i inventory.yml  -m win_ping

192.168.57.4 | SUCCESS => {
    "changed": false,
    "ping": "pong"

{% endhighlight %}
Onde:<br />
web é o grupo declarado no inventory.yml <br />
-i é para especificar o arquivo de inventory <br />
-m é para declarar qual modo irei utilizar.<br />

A partir desse ponto o nosso ambiente esta instalado, configurado e testado.

Ate a proxima.

[pws]:https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1

