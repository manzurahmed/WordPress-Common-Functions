GitHub Markdown syntax: https://help.github.com/articles/basic-writing-and-formatting-syntax/

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
1. এই ফাংশন সবসময়ই থিমের রুট ফোল্ডার থেকে টেমপ্লেট পার্ট খুঁজে থাকে।
2. থিমের টেমপ্লেট পার্টগুলোকে সবসময়ই এই ফাংশন দিয়ে লোড করতে হবে।
3. কিন্তু, ক্লাস ফাইল বা অন্যান্য ফাংশন ফাইলকে include_once বা require_once দিয়ে লোড করতে হবে।

# WPTD Class 3.16 Hasin Haider
সিঙ্গেল পোস্ট ভিউতে পোস্ট নেভিগেশন
```php
next_post_link();
previous_post_link();
```

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

# WPTD Class 3.19 Hasin Haider
পাসওয়ার্ড প্রটেকটেড পোস্ট ম্যানেজ করা
* Method 1: Write Code in index.php or where necessary
```php
if( !post_password_required() )
{
    the_excerpt();
}
else
{
    echo get_the_password_form();
}
```
অথবা
```php
if( post_password_required() )
{
    echo "This post is protected!";
}
else
{
    the_excerpt();
}
```

* Method 2: Apply filter for the_excerpt in functions.php file
```php
function alpha_the_excerpt( $excerpt )
{
	if( !post_password_required() )
	{
		return $excerpt;
	}
	else
	{
		echo get_the_password_form();
	}
}
add_filter( "the_excerpt", "alpha_the_excerpt");
```

* To omit the word "Protected: " from protected article's title, write another filter in functions.php file.
```php
function alpha_protected_title_change()
{
	return "%s";
}
add_filter( 'protected_title_format', "alpha_protected_title_change" );
```


# WPTD Class 3.21 Hasin Haider
* **External** JS File enque from functions.php file
```php
wp_enqueue_script( 'featherlight', get_template_directory_uri() . '/assets/js/featherlight.min.js', array('jquery'), '1.7.13', true );
```

# WPTD Class 3.22 Hasin Haider
* **Internal** JS File enque from functions.php file
```php
wp_enqueue_script( 'alpha-main', get_template_directory_uri() . '/assets/js/main.js', null, '0.0.1', true );
// OR, the file below can be used, but, get_theme_file_uri() function isn't available before 4.7 version of WordPress
wp_enqueue_script( 'alpha-main', get_theme_file_uri('/assets/js/main.js'), null, '0.0.1', true );
```
* Trick
We can popup the image in big size with jQuery.
Put the following code in main.js file in assets/js/main.js file.
```jquery
// This script taken all image's src and put the image URL in the href attr
$('.popup').each(function(){
    var image = $(this).find("img").attr("src");
    $(this).attr("href", image);
});
```

# WPTD Class 3.24 Hasin Haider
* Single page view and Page template
কোন পেজের জন্য আলাদা টেমপ্লেট ব্যবহার করতে হলে থিমের রুটে একটা নতুন PHP ফাইল বানাতে হবে। মনে করি, About পেজের জন্য নতুন টেমপ্লেট বানালাম about-page-template.php ফাইল।
এই ফাইলের ভিতরে প্রথমে লিখতে হবে:
```php
<?php
/*
 * Template Name: About Page Template
 */
?>
```

এরপর page.php ফাইলের কনটেন্ট কপি করে নিয়ে এসে এর মধ্যে প্রয়োজনীয় পরিবর্তন গুলো করে ফেলতে হবে।
