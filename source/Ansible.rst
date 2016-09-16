Ansible
=======
Esta é uma documentação que mostra como criar receitas no Ansible.
Ela foi feita com base na `documentação oficial <http://docs.ansible.com/ansible/index.html>`_.

Instalação
---------

No momento da instalação foi utilizado:

* **SO**: Centos 7.2
* **Ansible**: 2.1.1.0

Instalando::

    # yum install epel-release wget
    # yum install ansible

Inventários
-----------

O Ansible conecta nas máquinas de acordo com os endereços registrados no seu arquivo ``hosts``. Vide `documentação <http://docs.ansible.com/ansible/intro_inventory.html>`_.
Nesta configuração é possível definir hosts individuais ou grupos. Exemplo de um grupo chamado ``servidores``::

    vi /etc/ansible/hosts

    [servidores]
    192.168.25.14
    192.168.25.15



Ad-Hoc
------

Uma das formas para executar um comando remotamente pelo Ansible é adicionar nos hosts conhecidos do ssh (known_hosts) os hosts remotos e então executar comandos. Vide `documentação <http://docs.ansible.com/ansible/intro_adhoc.html>`_.
Para isto, execute o ssh para host de destino para que ele seja adicionado::

    ssh 192.168.25.14
    ....
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '192.168.25.14' (ECDSA) to the list of known hosts.
    root@192.168.25.14's password:

Agora é possível executar comandos remotamente. O comando será executado com base na lista de hosts do inventário.
Exemplo de execução no grupo ``servidores``::

    ansible servidores -k -a "uname -a"

    SSH password:
    192.168.25.14 | SUCCESS | rc=0 >>
    Linux servidor1.local 3.10.0-327.28.3.el7.x86_64 #1 SMP Thu Aug 18 19:05:49 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

    192.168.25.15 | UNREACHABLE! => {
        "changed": false,
        "msg": "Failed to connect to the host via ssh.",
        "unreachable": true
    }


Receitas - Playbook
-------------------

As playbooks são receitas pré-definidas em arquivos de configuração (formato YAML) a serem utilizadas nos hosts, com abordagem diferente ao
AD-Hoc, pois podem executar uma série de definições pré-determinadas. Vide documentação `playbooks <http://docs.ansible.com/ansible/playbooks.html>`_ e `YAML <http://docs.ansible.com/ansible/YAMLSyntax.html>`_.

    ansible-playbook servidores.yml -k

    SSH password:
    PLAY [servidores] **************************************************************

    TASK [setup] *******************************************************************
    ok: [192.168.25.14]
    fatal: [192.168.25.15]: UNREACHABLE! => {"changed": false, "msg": "Failed to connect to the host via ssh.", "unreachable": true}

    TASK [verifica versao do kernel] ***********************************************
    changed: [192.168.25.14]

    TASK [debug] *******************************************************************
    ok: [192.168.25.14] => {
        "uname.stdout_lines": [
            "Linux servidor1.local 3.10.0-327.28.3.el7.x86_64 #1 SMP Thu Aug 18 19:05:49 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux"
        ]
    }
    	to retry, use: --limit @/root/.ansible-retry/servidores.retry

    PLAY RECAP *********************************************************************
    192.168.25.14              : ok=3    changed=1    unreachable=0    failed=0
    192.168.25.15              : ok=0    changed=0    unreachable=1    failed=0

..note: Quando há falhas, um arquivo `retry <http://docs.ansible.com/ansible/intro_configuration.html#retry-files-enabled>`_ é salvo. Por padráo é necessário definir o caminho ou desativar a função.
