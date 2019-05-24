# 14 WordPress Performance Optimization to do Without Plugin

https://geekflare.com/wordpress-performance-optimization-without-plugin/#Remove-Query-Strings

### Disable Dashicons on Front-end

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
### Disable or Limit Post Revisions

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
