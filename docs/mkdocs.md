# Guia de Instalação do MkDocs

Escrito por: **Edigar de Almeida Carvalho**

## Introdução ao MkDocs

O MkDocs é um gerador de sites estáticos rápido e simples, ideal para criar documentação de projetos. Este guia irá orientá-lo no processo de instalação do MkDocs no Ubuntu. Vamos começar!

Antes de iniciar a instalação, certifique-se de que você tem o Python instalado. Verifique a versão do Python e do pip digitando os seguintes comandos no terminal:

```sh
python --version
pip --version
```

## 1 - Instalação do MkDocs

Para instalar o MkDocs, utilize o pip, o instalador de pacotes do Python. Execute o comando abaixo no terminal:

```sh
pip install mkdocs
```

Digite sua senha e pressione Enter. A instalação do MkDocs no seu sistema Ubuntu levará alguns minutos. Aqui, estamos utilizando o Ubuntu versão 22.04 LTS.

Verifique se o MkDocs foi instalado com sucesso verificando a versão instalada:

```sh
mkdocs --version
```

## 2 - Criando um Projeto de Documentação

Para criar um projeto de documentação usando o MkDocs, navegue até o local onde deseja criar o projeto no terminal. Aqui, vamos criar um projeto na área de trabalho:

```sh
cd ~/Desktop
mkdocs new nome_do_projeto
```

Substitua "nome_do_projeto" pelo nome desejado para seu projeto. 

Agora, abra a nova pasta criada e liste todos os arquivos do MkDocs:

```sh
cd nome_do_projeto
ls
```

## 3 - Servindo a Documentação Localmente

Para iniciar o servidor de desenvolvimento do MkDocs e visualizar sua documentação localmente, utilize o comando:

```sh
mkdocs serve
```

O terminal exibirá um link localhost. Copie e cole esse link em qualquer navegador web para visualizar a documentação.

## 4 - Construindo a Documentação

Se você deseja construir a documentação para distribuição, utilize o comando:

```sh
mkdocs build
```

Este comando gerará uma versão estática da sua documentação, pronta para ser hospedada em qualquer servidor web.

Parabéns! Você instalou com sucesso o MkDocs no seu sistema Ubuntu e criou um projeto básico de documentação. Lembre-se de que o MkDocs facilita a manutenção e organização da documentação do seu projeto.

Se você achou este tutorial útil, não se esqueça de curtir, compartilhar e se inscrever para mais conteúdos. Obrigado por assistir e boa documentação!

![Imagem de Exemplo](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/MkDocs.png/1024px-MkDocs.png)

---

Espero que este guia seja útil! Se precisar de mais alguma coisa, estou à disposição.