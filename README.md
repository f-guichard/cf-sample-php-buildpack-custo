## CF PHP Buildpack

A bootstrap custo of CF PHP Buildpack

```bash
git clone https://github.com/cloudfoundry/php-buildpack.git
cd php-buildpack/
git submodule update --init
rvm install ruby 2.2.3
rvm use ruby 2.2.3
gem install bundle
sudo apt-get -y install libgmp3-dev
BUNDLE_GEMFILE=cf.Gemfile bundle
```

  - Ajout de type mime spécifiques DFY
    - ```
    vim defaults/config/httpd/extra/httpd-mime.conf
    AddType application/dfy-type .dfy
    AddType application/hbx-type .hbx
    AddType application/mfy-type .mfy
    ```
  - Modification properties
    - ```
    vim defaults/options.json
    ```
  - Ajout de regles de rewrite communes
    - ```
    vim defaults/config/httpd/extra/httpd-directories.conf
    RewriteEngine On
    RewriteRule     "^nano*$"        "reponserobot.php?param=rewriteactive"     [NC,L]  #Renvoie le rel path si l'url contient gourdet
    RewriteRule     "^admin*$"          "alertesecu.php?param=scriptkiddie"     [NC,L]
    ```
```bash
BUNDLE_GEMFILE=cf.Gemfile bundle exec buildpack-packager --cached
cf create-buildpack dfy_php_buildpack php_buildpack-v4.3.1.zip 7
cf push my_app -b dfy_php_buildpack
```

