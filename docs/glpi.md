# *Guia de Instalação do GLPI*

Escrito por: **Edigar de Almeida Carvalho**

## Introdução ao GLPI

O GLPI é uma ferramenta de código aberto essencial para o gerenciamento de ativos de TI e suporte técnico. Antes de iniciar a instalação, é crucial garantir que todos os requisitos estejam atendidos. Recomenda-se utilizar um servidor Linux, preferencialmente o Ubuntu 22.04 LTS, juntamente com um servidor web como o Apache, PHP e um sistema de gerenciamento de banco de dados como o MariaDB. Embora este guia se concentre no Ubuntu, as instruções podem ser adaptadas para outras distribuições Linux. Abaixo uma imagem de um dashboard na ferramenta :

![Image title](https://upload.wikimedia.org/wikipedia/commons/9/93/Glpi_screenshot_en.png)

## 1 - Instalação dos Componentes 

Antes de começar, assegure-se de que seu servidor esteja atualizado:

```sh
sudo apt update && sudo apt upgrade
```

⚠️ **Atenção:** Caso o firewall esteja ativado, é necessário permitir conexões SSH, HTTP e HTTPS conforme suas políticas de segurança.

Instale os componentes necessários incluindo Apache 2, MariaDB Server, PHP e extensões relacionadas:

```sh
sudo apt install -y apache2 php php-{apcu,cli,common,curl,gd,imap,ldap,mysql,xmlrpc,xml,mbstring,bcmath,intl,zip,redis,bz2} libapache2-mod-php php-soap php-cas
sudo apt install -y mariadb-server
```

Após a instalação dos componentes, prossiga com os passos de 2 a 6.

## 2 - Configuração do Banco de Dados

Por padrão, o MariaDB não possui uma senha definida para o usuário root e possui configurações padrão que devem ser ajustadas.

### Melhorias na Segurança do MariaDB

```sh
sudo mysql_secure_installation
```

**Recomendações Mínimas:**
- Alterar a senha do root
- Remover usuários anônimos
- Desativar login remoto do root
- Remover banco de dados de teste
- Recarregar tabelas de privilégios

Como o GLPI é utilizado globalmente, é recomendável permitir que o serviço do GLPI acesse informações de fuso horário no banco de dados MySQL.

```sh
sudo mysql_tzinfo_to_sql /usr/share/zoneinfo | sudo mysql mysql
```

### Criação de Usuário e Banco de Dados para o GLPI

```sh
sudo mysql -uroot -p
CREATE DATABASE glpi;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY 'suasenhaforte';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpi'@'localhost';
GRANT SELECT ON `mysql`.`time_zone_name` TO 'glpi'@'localhost';
FLUSH PRIVILEGES;
```

⚠️ **Atenção:** Escolha uma senha forte para o usuário do serviço, utilizado pelo GLPI para acessar e gerenciar o banco de dados.

## 3 - Preparação dos Arquivos para Instalação do GLPI

Depois de instalar os componentes e configurar o banco de dados, faça o download da última versão do GLPI e extraia na raiz do Apache.

### Download da Última Versão do GLPI

```sh
cd /var/www/html
sudo wget https://github.com/glpi-project/glpi/releases/download/10.0.15/glpi-10.0.15.tgz
sudo tar -xvzf glpi-10.0.15.tgz
```

### Estrutura de Arquivos Padrão

Organize os arquivos do GLPI em diretórios específicos seguindo o padrão FHS (Filesystem Hierarchy Standard):

- `/etc/glpi`: Arquivos de configuração (ex.: `config_db.php`, `config_db_slave.php`)
- `/var/www/html/glpi`: Código-fonte servido pelo Apache
- `/var/lib/glpi`: Arquivos variáveis (sessões, documentos, cache, cron, plugins, etc.)
- `/var/log/glpi`: Arquivos de log

Para garantir a localização dos arquivos, especifique esses diretórios em dois arquivos diferentes.

### Criação do Arquivo Downstream

```sh
sudo vim /var/www/html/glpi/inc/downstream.php
```

Adicione o seguinte conteúdo:

```php
<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}
```

Mova as pastas para seus respectivos diretórios:

```sh
sudo mv /var/www/html/glpi/config /etc/glpi
sudo mv /var/www/html/glpi/files /var/lib/glpi
sudo mv /var/lib/glpi/_log /var/log/glpi
```

Depois de definir `GLPI_CONFIG_DIR` no `downstream.php`, vá para `/etc/glpi` e crie o arquivo `local_define.php`.

### Criação do Arquivo Local Define

```sh
sudo vim /etc/glpi/local_define.php
```

Adicione o seguinte conteúdo:

```php
<?php
define('GLPI_VAR_DIR', '/var/lib/glpi');
define('GLPI_DOC_DIR', GLPI_VAR_DIR);
define('GLPI_CRON_DIR', GLPI_VAR_DIR . '/_cron');
define('GLPI_DUMP_DIR', GLPI_VAR_DIR . '/_dumps');
define('GLPI_GRAPH_DIR', GLPI_VAR_DIR . '/_graphs');
define('GLPI_LOCK_DIR', GLPI_VAR_DIR . '/_lock');
define('GLPI_PICTURE_DIR', GLPI_VAR_DIR . '/_pictures');
define('GLPI_PLUGIN_DOC_DIR', GLPI_VAR_DIR . '/_plugins');
define('GLPI_RSS_DIR', GLPI_VAR_DIR . '/_rss');
define('GLPI_SESSION_DIR', GLPI_VAR_DIR . '/_sessions');
define('GLPI_TMP_DIR', GLPI_VAR_DIR . '/_tmp');
define('GLPI_UPLOAD_DIR', GLPI_VAR_DIR . '/_uploads');
define('GLPI_CACHE_DIR', GLPI_VAR_DIR . '/_cache');
define('GLPI_LOG_DIR', '/var/log/glpi');
```

## 4 - Permissões

Configure as permissões adequadas para sua instalação do GLPI:

```sh
sudo chown root:root /var/www/html/glpi/ -R
sudo chown www-data:www-data /etc/glpi -R
sudo chown www-data:www-data /var/lib/glpi -R
sudo chown www-data:www-data /var/log/glpi -R
sudo chown www-data:www-data /var/www/html/glpi/marketplace -Rf
sudo find /var/www/html/glpi/ -type f -exec chmod 0644 {} \;
sudo find /var/www/html/glpi/ -type d -exec chmod 0755 {} \;
sudo find /etc/glpi -type f -exec chmod 0644 {} \;
sudo find /etc/glpi -type d -exec chmod 0755 {} \;
sudo find /var/lib/glpi -type f -exec chmod 0644 {} \;
sudo find /var/lib/glpi -type d -exec chmod 0755 {} \;
sudo find /var/log/glpi -type f -exec chmod 0644 {} \;
sudo find /var/log/glpi -type d -exec chmod 0755 {} \;
```

## 5 - Configuração do Servidor Web

Para garantir que o GLPI funcione sem URLs complexas, configure um nome DNS para seu servidor e crie um Virtual Host.

### Criação de um Virtual Host para o GLPI

Crie um arquivo em `/etc/apache2/sites-available/glpi.conf`:

```sh
sudo vim /etc/apache2/sites-available/glpi.conf
```

Adicione o seguinte conteúdo:

```apache
<VirtualHost *:80>
    ServerName suaglpi.seudominio.com
    DocumentRoot /var/www/html/glpi/public
    <Directory /var/www/html/glpi/public>
        Require all granted
        RewriteEngine On
        RewriteCond %{HTTP:Authorization} ^(.+)$
        RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>
</VirtualHost>
```

### Habilitar o Virtual Host

Desabilite a configuração de site padrão do Apache, ative o módulo de rewrite e recarregue o novo Virtual Host:

```sh
sudo a2dissite 000-default.conf
sudo a2enmod rewrite
sudo a2ensite glpi.conf
sudo systemctl restart apache2
```

### Configuração do Arquivo PHP.ini

Edite o arquivo `php.ini`:

```sh
sudo vim /etc/php/8.1/apache2/php.ini
```

Altere os seguintes parâmetros conforme necessário:

```ini
upload_max_filesize = 20M
post_max_size = 20M
max_execution_time = 60
max_input_vars = 5000
memory_limit = 256M
session.cookie_httponly = On
date.timezone = America/Sao_Paulo
```

Consulte a [lista oficial de fusos horários suportados pelo PHP](https://www.php.net/manual/en/timezones.php) para o seu fuso horário.

## 6 - Instalação Web


Uma vez que a instalação e configuração das dependências estiverem concluídas, continue o processo de instalação em um navegador web com acesso a este mesmo servidor. Abra um navegador web e digite o registro DNS que você criou para este servidor. 🌐

Você deverá ver uma tela para selecionar uma linguagem :

- Selecione a linguagem

![Image title](https://www.atlantic.net/wp-content/uploads/2024/03/p1-4.png)

- Aceite os termos 

![Image title](https://i.postimg.cc/ncsrVV5S/image.png)

- Clique em Install para instalar

![Image title](https://verdanadesk.com/wp-content/uploads/2022/04/image-3-1024x367.png)

- Verifique se os itens da checklist estão corretos e clique em continuar

![Image title](https://verdanadesk.com/wp-content/uploads/2023/04/image-1.png)

- Conecte-se ao banco de dados e clique em continuar

![Image title](https://miro.medium.com/v2/resize:fit:1400/0*odOfYm-ytjTQju6b.jpg)

- Selecione o banco de dados correto

![Image title](https://i.postimg.cc/sgR6fxx3/image.png)

- Siga clicando em continuar até finalizar a instalação e aparecer "User GLPI"

![Image title](https://i.postimg.cc/8kvkxXrx/image.png)

- Após concluir todos os passos, você verá a tela de login do GLPI. Parabéns, você finalizou a instalação do GLPI! Para começar a usar a ferramenta, basta fazer login. 🥳🥳🥳

![Image title](https://hedgedoc.teclib.com/uploads/upload_0e2f7386041260c85df9c4111e4ab347.png)



