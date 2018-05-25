# WordPress-Common-Functions
This is a repository of WordPress functions that I encountered during WordPress Theme and Plugin development tutorials

# WPTD Class 3.12 Hasin Haider
কোন পোস্টের কমেন্ট এনাবল/ডিজাবল স্টেট ভ্যালু চেক করার পর, কমেন্ট টেমপ্লেট লোড করা আবশ্যক।
```php
if(comments_open()): ?>
<div class="col-md-10 offset-md-1">
<?php
comments_template();
?>
</div>
<?php endif; ?>
```

# WPTD Class 3.15 Hasin Haider
get_template_part()
সবসময়ই থিমের রুট ফোল্ডার থেকে টেমপ্লেট পার্ট খুঁজে থাকে।

# WPTD Class 3.16 Hasin Haider
সিঙ্গেল পোস্ট ভিউতে পোস্ট নেভিগেশন
next_post_link();
previous_post_link();

# WPTD Class 3.17 Hasin Haider
উইজেট এরিয়া রেজিস্টার করা এবং সিঙ্গেল পোস্ট ভিউ তে সাইডবার দেখানো
```php
register_sidebar(
		array(
			'name'					=> __( "Single Post Sidebar", 'alpha' ),
			'id'					=> 'sidebar-1',
			'description'			=> __( 'Right Sidebar', 'alpha' ),
			'before_widget'			=> '<section id="%1$s" class="widget %2$s">',
			'after_widget'			=> '</section>',
			'before_title'			=> '<h2 class="widget-title">',
			'after_title'			=> '</h2>'
		)
	);
add_action('widgets_init', 'alpha_sidebar');
```

সাইডবার দেখানোর জন্য:
```php
<?php
if(is_active_sidebar( 'sidebar-1' ))
{
    dynamic_sidebar( 'sidebar-1' );
}
?>
```
