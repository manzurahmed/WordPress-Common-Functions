# 14 WordPress Performance Optimization to do Without Plugin

https://geekflare.com/wordpress-performance-optimization-without-plugin/#Remove-Query-Strings

## Disable Dashicons on Front-end

Dashicons are utilized in the admin console, and if not using them to load any icons on front-end then you may want to disable it. By adding below, dashicons.min.css will stop loading on front-end.

```php
function wpdocs_dequeue_dashicon() {
        if (current_user_can( 'update_core' )) {
            return;
        }
        wp_deregister_style('dashicons');
}
add_action( 'wp_enqueue_scripts', 'wpdocs_dequeue_dashicon' );
```

## Disable Auto-Save

```
define( 'AUTOSAVE_INTERVAL', 60*60*60*24*365 ); // Set autosave interval to 1x per year
```

## Empty Trash Now
```
define( 'EMPTY_TRASH_DAYS',  0 ); // Empty trash now: Zero days
```

define( 'WP_POST_REVISIONS', false ); // Do not save andy revisions

## Disable or Limit Post Revisions

Post revisions in WordPress are not new and helpful to restore the post if browser crash or lose the network. But ask yourself, how many times did it happen?

By default, WordPress will save revisions for each draft or published a post, and this can bloat the database. You may either choose to disable it entirely or limit the number of revisions to be saved.

Add the following in wp-config.php file

**To disable post revisions**

```
define('WP_POST_REVISIONS', false);
```

**To limit the number**

Let’s say limit to keep max two revisions
```
define('WP_POST_REVISIONS', 2);
```
Note: this must be **above ABSPATH** line else it won’t work.

## Hide WordPress Version

This doesn’t help in performance but more to mitigate information leakage vulnerability. By default, WordPress adds meta name generator with the version details which is visible in source code and HTTP header.

To remove the WP version, add below code.
```
remove_action( 'wp_head', 'wp_generator' ) ;
```

## Disable XML-RPC

Do you have a requirement to use WordPress API (XML-RPC) to publish/edit/delete a post, edit/list comments, upload file? Also having XML-RPC enabled and not hardened properly may lead to DDoS & brute force attacks.

If you don’t need then disable it by adding below.
```
add_filter('xmlrpc_enabled', '__return_false');
```

## Disable Emoticons

Remove extra code related to emojis from WordPress which was added recently to support emoticons in an older browser.
```
remove_action('wp_head', 'print_emoji_detection_script', 7);
remove_action('wp_print_styles', 'print_emoji_styles');
remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
remove_action( 'admin_print_styles', 'print_emoji_styles' );
```

## Remove RSD Links

RSD (Really Simple Discovery) is needed if you intend to use XML-RPC client, pingback, etc. However, if you don’t need pingback or remote client to manage post then get rid of this unnecessary header by adding the following code.
```
remove_action( 'wp_head', 'rsd_link' ) ;
```

