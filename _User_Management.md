# User Management

## Get current user and check if User is logged in

```
// https://codex.wordpress.org/wp_get_current_user
$current_user = wp_get_current_user();

// If user NOT logged in
if ( '' == $current_user->ID ) {
  // Code goes here
}
```
