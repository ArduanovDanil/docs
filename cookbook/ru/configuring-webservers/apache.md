# Конфигурирование веб-серверов: Apache

Используйте следующую конфигурацию в файле Apache `httpd.conf` или в
конфигурации виртуального хоста. Обратите внимание, что `path/to/app/public`
следует заменить на фактический путь к `app/public`.

```apacheconfig
# Set document root to be "app/public"
DocumentRoot "path/to/app/public"

<Directory "path/to/app/public">
    # use mod_rewrite for pretty URL support
    RewriteEngine on
    
    # if $showScriptName is false in UrlManager, do not allow accessing URLs with script name
    RewriteRule ^index.php/ - [L,R=404]
    
    # If a directory or a file exists, use the request directly
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    
    # Otherwise forward the request to index.php
    RewriteRule . index.php
    
    SetEnv APP_ENV dev

    # ...other settings...
</Directory>
```

Если у вас есть `AllowOverride All`, вы можете добавить файл `.htaccess` со
следующей конфигурацией вместо использования `httpd.conf`:

```apacheconfig
# use mod_rewrite for pretty URL support
RewriteEngine on

# if $showScriptName is false in UrlManager, do not allow accessing URLs with script name
RewriteRule ^index.php/ - [L,R=404]

# If a directory or a file exists, use the request directly
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

# Otherwise forward the request to index.php
RewriteRule . index.php

SetEnv APP_ENV dev

# ...other settings...
```

В примере выше обратите внимание на использование `SetEnv`. Поскольку шаблон
приложения Yii3 использует переменные окружения, это возможное место для их
установки. В рабочей среде не забудьте установить `APP_ENV` в `prod`.
