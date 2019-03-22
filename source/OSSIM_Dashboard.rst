Painel de Segurança com OSSIM e PowerBI
===============================
Para quem usa a ferramenta `AlienVault OSSIM <https://www.alienvault.com/products/ossim>`_ e acha um tanto complicado gerar relatórios utilizando os recursos nativos, é possível coletar os dados e montar painéis no `Microsoft PowerBI Desktop <https://powerbi.microsoft.com>`_, conforme sua necessidade. Além de uma flexibilidade no tratamento dos dados, análises tornam-se rápidas e precisas.


.. figure:: Painel_OSSIM_PBI.png
    :scale: 60 %
    :align: center
    :alt: Painel do OSSIM no PowerBI

    Exemplo de um *dashboard* no PowerBI com dados do OSSIM.



Em resumo, para alcançar isto é necessário:

* **OSSIM** - ferramenta instalada com varreduras já realizadas obviamente

* **Base de Dados** - possuir acesso a base de dados do OSSIM

* **PowerBI** - conhecer a ferramenta para criar relacionamentos entre dados e personalizar painéis



Coleta de Dados via Power BI
--------------
Os procedimentos para a coleta dos dados via Power BI.

.. note:: **Usuário/Senha** - recomenda-se uma conta somente de leitura para acesso a base.

 **MySQL** - foi a base de dados utilizada neste tutorial.


* **Driver de Conexão com Banco** - para que o Power BI consiga ler dados do banco de dados é necessário instalar um conector:

  * **Conector MySQL** - https://dev.mysql.com/downloads/connector/net/


* **Conexão com a Base Local Restrita** - caso tenha acesso a base pule essa etapa, porém se deseja acessar a base (restrita somente para o servidor do OSSIM) via túnel SSH, é necessário:

  * **Resolução de Nome** - alterar a configuração do banco de dados (``/etc/mysql/my.cnf``), alterando o parâmetro ``skip_name_resolve`` para ``#skip_name_resolve``.

  * **Túnel SSH** - para acessar a base, que está disponível apenas localmente no servidor, faça uma conexão SSH com o servidor tunelando a porta do MySQL


* **Tabelas e Colunas** - ao conectar na base de dados ``alienvault``, foram selecionadas algumas tabelas continham informações relevantes (para mim), sendo elas:

  * **alienvault_vuln_nessus_category**
  * **alienvault_vuln_nessus_family**
  * **alienvault_vuln_nessus_plugins**
  * **alienvault_vuln_nessus_results**



.. figure:: OSSIM_Tabelas.png
    :scale: 80 %
    :align: center
    :alt: Tabelas do OSSIM no PowerBI

    Tabelas do OSSIM importadas no PowerBI.



Depois de importado os dados e analisado seu conteúdo, observa-se as PKs (chaves primárias) de forma a criar o relacionamento entre as tabelas. A figura abaixo, exibe a visão final de como ficaram os relacionamentos entre as tabelas e algumas (tabelas e colunas) criadas adicionalmente, explicadas logo a seguir.

.. figure:: OSSIM_Relacionamentos.png
    :scale: 80 %
    :align: center
    :alt: Relacionamento de tabelas no PowerBI

    Relacionamento de tabelas no PowerBI.

Foram criadas algumas tabelas e colunas para que fosse possível atender a certas necessidades, sendo elas:

* **Data do Scan** - como a data da varredura estava em um formato que não era possível hierarquizá-las, foi criada uma nova coluna na tabela ``alienvault_vuln_nessus_results`` na sintaxe DAX::


    Data_Scan = DATE(
                     LEFT('alienvault vuln_nessus_results'[scantime];4);          //Ano
                     RIGHT(LEFT('alienvault vuln_nessus_results'[scantime];6);2); //Mês
                     RIGHT(LEFT('alienvault vuln_nessus_results'[scantime];8);2)) //Dia


* **IPs por SubRede** - para que fosse possível analisar vulnerabilidade por rede. Foi criada outra coluna na tabela ``alienvault vuln_nessus_results`` utilizando sintaxe DAX que exclui o último octeto::

    subnet = PATHITEM(SUBSTITUTE('alienvault vuln_nessus_results'[hostIP];".";"|");1) & // Primeiro Octeto
             "." &
             PATHITEM(SUBSTITUTE('alienvault vuln_nessus_results'[hostIP];".";"|");2) & // Segundo Octeto
             "." &
             PATHITEM(SUBSTITUTE('alienvault vuln_nessus_results'[hostIP];".";"|");3) & // Terceiro Octeto
             "."

.. note:: Para segmentação de redes com máscaras mais fechadas, por exemplo /25, um tratamento correto deve ser feito.


De forma a permitir filtros por IPs, riscos com nomes personalizados e também poder agrupar IPs por sub-redes, de acordo com o ambiente (exemplo: Rede A, Rede B, Rede C), foram inseridas tabelas adicionais, criando-se relacionamentos com as da base do OSSIM.

* **dIPs** - criada a tabela ``dIPs`` contendo todos IPs do ambiente. Isto foi necessário para que o filtro cruzado com a tabela ``dSubRedes`` fosse possível, isto é, que os IPs pudessem ser agrupado em uma rede específica.

* **dSubRedes** - criada a tabela ``dSubRedes``, com base nos dados extraídos da ferramenta IPAM, na qual continha todas as definições de sub-redes, permitindo que filtros por sub-rede no PowerBI fossem feitos

* **dRisco** - a tabela de dimensão ``dRisco`` permitiu personalizar a descrição dos riscos.

O mapa de relacionamentos ficou assim:

.. figure:: OSSIM_Relacionamentos2.png
    :scale: 80 %
    :align: center
    :alt: Relacionamento de tabelas e suas respectivas chaves no PowerBI

    Relacionamento de tabelas e suas respectivas chaves no PowerBI.


Agora é ajustar o painel de vulnerabilidades no PowerBI e extrair informaçoes do OSSIM e, principalmente, manter seu ambiente atualizado e seguro.

.. figure:: Painel_OSSIM_PBI2.png
    :scale: 80 %
    :align: center
    :alt: Painel do OSSIM no PowerBI

    Outro exemplo de um *dashboard* no PowerBI com dados do OSSIM.
