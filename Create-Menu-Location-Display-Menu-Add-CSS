# WPTD Class 3.20

* মেনু তৈরীর ধাপ গুলো হল:
1. functions.php তে মেনু লোকেশন তৈরী করা (register_nav_menu)
2.  মেনু ডিসপ্লে করা (wp_nav_menu)
3. Bootstrap 4 এ লিস্ট আইটেম কে horizontal alignment করতে হলে filter দিয়ে nav_menu_css_class হুক কে কলব্যাক ফাংশন দিয়ে প্রয়োজনীয় css যুক্ত করতে হয়।

* মেনু লোকেশন তৈরী করা
```php
register_nav_menu( 'topmenu', __('Top Menu', 'alpha') );
```

* মেনু ডিসপ্লে করা
```php
<?php
wp_nav_menu(
    array(
        'theme_location' => 'topmenu',
        'menu_id' => 'topmenucontainer',
        'menu_class' => 'list-inline text-center'
    )
);
?>
```
