git clone git@code.aliyun.com:aiji66/www.git


安装composer
```
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"  下载安装脚本composer-setup.php到当前目录
php composer-setup.php    执行安装过程
php -r "unlink('composer-setup.php');"   删除安装脚本

mv composer.phar /usr/local/bin/composer

composer config -g repo.packagist composer https://packagist.phpcomposer.com    修改镜像配置
```


使用composer搭建
```
composer global require "fxp/composer-asset-plugin:^1.3.1"
composer create-project --prefer-dist yiisoft/yii2-app-advanced advanced
```