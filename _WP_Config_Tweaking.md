# WordPress Configuration Tweaking

## Wordpress admin login cookies blocked error after moving servers

Add the following line in your **wp-config.php** file.
```
define('COOKIE_DOMAIN', false);
```

```
define('ADMIN_COOKIE_PATH', '/');
define('COOKIE_DOMAIN', '');
define('COOKIEPATH', '');
define('SITECOOKIEPATH', '');
```
