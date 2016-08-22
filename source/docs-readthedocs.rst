Utilizando o "Read the Docs"
===============================
Escrever é uma tarefa difícil. Isso torna a documentação uma arte. Para isto, algumas soluções a auxiliam. Uma delas é o https://readthedocs.org/.

* GitHub
* Read the Docs
* Sphinx


Linux
-----------
Para este procedimento foi utilizado um ``CentOS 7``.::

        yum install git python-sphinx
        git clone https://github.com/conta/repositorio
        cd repositorio
        git init
        sphinx-quickstart
        make html
        git add --all
        git commit -a -m "A arte de documentar"
        git push
