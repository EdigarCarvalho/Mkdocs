# Guia de Instalação e Uso do MkDocs 📚

Escrito por: **Edigar de Almeida Carvalho**

## Relembrando

O MkDocs é um gerador de sites estáticos rápido e simples, ideal para criar documentação de projetos. Este guia irá orientá-lo no processo de instalação do MkDocs no Ubuntu, criação de um projeto de documentação e configuração básica usando um arquivo YAML.

## 1 - Verificando Requisitos ✅

Antes de iniciar a instalação, certifique-se de que você tem o Python e o pip instalados. Verifique a versão do Python e do pip digitando os seguintes comandos no terminal:

```sh
python --version
pip --version
```

## 2 - Instalando o MkDocs 🛠️

Para instalar o MkDocs, utilize o pip, o instalador de pacotes do Python. Execute o comando abaixo no terminal:

```sh
pip install mkdocs
```

Digite sua senha e pressione Enter. A instalação do MkDocs no seu sistema Ubuntu levará alguns minutos. Aqui, estamos utilizando o Ubuntu versão 22.04 LTS.

Verifique se o MkDocs foi instalado com sucesso verificando a versão instalada:

```sh
mkdocs --version
```

## 3 - Criando um Projeto de Documentação 📂

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

## 4 - Configurando o Projeto com `mkdocs.yml` ⚙️

O arquivo `mkdocs.yml` ajuda o MkDocs a entender como construir o site de documentação. Aqui está um exemplo básico de configuração:

```yaml
site_name: Meu Projeto
theme:
  name: mkdocs
plugins:
  - search
  - mkdocstrings
nav:
  - Home: index.md
  - Sobre: about.md
```

### Explicação dos Campos:

- **site_name**: Define o título do site.
- **theme**: Define o tema do site (geralmente instalado com pip).
- **plugins**: Lista os plugins utilizados (geralmente instalado com pip).
- **nav**: Define a navegação do site.

### Instalando e Ativando um Tema 🎨

#### Instalando o Tema

Para instalar o tema "simple-blog", tema que eu utilizei, use o pip:

```sh
pip install mkdocs-simple-blog
```

#### Ativando o Tema

Após instalar o tema, edite o arquivo `mkdocs.yml` e defina o nome do tema para "simple-blog":

```yaml
theme:
  name: simple-blog
```
Para mais informações sobre o tema, consulte a [documentação do tema](https://fernandocelmer.github.io/mkdocs-simple-blog/).


## 5 - Servindo a Documentação Localmente 🌍

Para iniciar o servidor de desenvolvimento do MkDocs e visualizar sua documentação localmente, utilize o comando:

```sh
mkdocs serve
```

O terminal exibirá um link localhost. Copie e cole esse link em qualquer navegador web para visualizar a documentação. Você deverá ver algo parecido com a imagem:

![Image title](https://www.mkdocs.org/img/screenshot.png)

## 6 - Construindo a Documentação 🏗️

Se você deseja construir a documentação para distribuição, utilize o comando:

```sh
mkdocs build
```

Este comando gerará uma versão estática da sua documentação, pronta para ser hospedada em qualquer servidor web. A pasta `site` conterá todos os arquivos HTML e recursos necessários.

## 7 - Publicando a Documentação 🚀

O MkDocs possui um mecanismo para publicar diretamente a documentação no GitHub Pages. Para isso, seu projeto precisa estar vinculado a um repositório no GitHub:

1. Inicie um repositório Git no diretório do seu projeto, se ainda não estiver versionado:
   ```sh
   git init
   ```
2. Adicione seu repositório remoto:
   ```sh
   git remote add origin <URL_DO_SEU_REPOSITORIO>
   ```
3. Faça o deploy da sua documentação para o GitHub Pages:
   ```sh
   mkdocs gh-deploy
   ```

Espero que este guia seja útil! Se precisar de mais alguma coisa, estou à disposição.
