---
layout: post
title:  "Ansible For Windows - Install"
date:   2019-01-26 22:55
categories: [Ansible, Windows]
---

Nesses posts inicias ireis mostrar como configurar e trabalhar com o Ansible em ambiente Windows.
Com relação ao ambiente utilizado, estou executando 2 VMs no vagrant, segue vagrantfile.

{% highlight ruby %}

Vagrant.configure("2") do |config|
    config.vm.define "control" do |ctl|
        ctl.vm.box = "geerlingguy/centos7"
        ctl.vm.hostname = "ansible"
        ctl.vm.network "private_network", ip: "192.168.57.2"
        ctl.vm.provider "virtualbox" do |vb|
            vb.memory = 1024
        end
    end
    config.vm.define "webserver01" do |web01| 
        web01.vm.box = "jptoto/Windows2012R2"
        web01.vm.hostname = "webserver01"
        web01.vm.network "private_network", ip: "192.168.57.4"
        web01.vm.provider "virtualbox" do |vb|
            vb.memory = 1024
            vb.cpus = 2
        end
    end

end
{% endhighlight %}

Não vou entrar em detalhes a respeito do funcionamento do Vagrant.
Apos subir o ambiente, proximo passo é instalar o pacotes de suporte.

{% highlight ruby %}
  pip install markupsafe
  pip install xmltodict
  pip install pywinrm
{% endhighlight %}

Estou realizando a instalação do Ansible pelo pip mas poderia ser por qualquer outra forma.

{% highlight ruby %}
  pip install Ansible
{% endhighlight %}

Para verificar que tudo esta OK, so executar o ansible --version.

{% highlight ruby %}
  [vagrant@ansible ~]$ ansible --version
  ansible 2.4.2.0
    config file = /etc/ansible/ansible.cfg
    configured module search path = [u'/home/vagrant/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python2.7/site-packages/ansible
    executable location = /usr/bin/ansible
    python version = 2.7.5 (default, Oct 30 2018, 23:45:53) [GCC 4.8.5 20150623 (Red Hat 4.8.5-36)]
{% endhighlight %}

Com isso ja temos Ansible instalado e pronto para "brincar".