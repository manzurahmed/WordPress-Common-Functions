# How to Properly Move WordPress from HTTP to HTTPS (Beginner’s Guide)

https://www.wpbeginner.com/wp-tutorials/how-to-add-ssl-and-https-in-wordpress/

## Setup SSL/HTTPS in WordPress Manually

First, you need to visit Settings » General page. From here you need to update your WordPress and site URL address fields by replacing http with https.

### General Settings

- WordPress Address (URL): https://www.example.com
- Site Address (URL): https://www.example.com

Don’t forget to click on the ‘Save changes’ button to store your settings.

Once the settings are saved, WordPress will log you out, and you will be asked to re-login.

Next, you need to set up WordPress redirects from HTTP to HTTPS by adding the following code to your .htaccess file.

```
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</IfModule>
```

If you are on nginx servers (most users are not), then you would need to add the following code to redirect from HTTP to HTTPS in your configuration file:

```
server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://example.com$request_uri;
}
```

Don’t forget to replace example.com with your own domain name.

Simply add the following code above the “That’s all, stop editing!” line in your wp-config.php file:

```
    define('FORCE_SSL_ADMIN', true);
```

Once you do this, your website is now fully setup to use SSL / HTTPS, but you will still encounter mixed content errors.

### Fixing Mixed Content in WordPress Database

You can easily do this by installing and activating the Better Search Replace plugin. For more details, see our step by step guide on how to install a WordPress plugin.

