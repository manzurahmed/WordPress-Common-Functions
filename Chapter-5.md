# Chapter 5

## ৫.১ সিঙ্গেল পোস্টের পেজিনেশন

একটা সিঙ্গেল **বড় পোস্ট** কে একাধিক পেজে বিভক্ত করতে পোস্টের মধ্যে Text মুড এ গিয়ে যেখানে যেখানে পেজ তৈরী করা দরকার, সেখানে সেখানে
```html
<!--nextpage-->
```
লিখতে হবে। এবার single.php ফাইলে এসে the_content(); এর নীচে লিখতে হবে:

```php
wp_link_pages();
```

ব্যস! এই টুকুই যথেষ্ট।

## ৫.২ পোস্ট ফরম্যাট নিয়ে আলোচনা

```php
add_theme_support( 'post-formats', array( 'aside', 'gallery' ) );
```

বিস্তারিত জানতে: https://codex.wordpress.org/Post_Formats

aside, gallery, link, image, quote, status, video, audio, chat

functions.php ফাইলে wp_enqueue_scripts হুকের কলব্যাক ফাংসনে DashIcons এর সিএসএস কে এনকিউ করে নিতে হবে।

```php
wp_enqueue_style( 'dashicons');
```

Dash Icons URL: https://developer.wordpress.org/resource/dashicons/#phone

## ৫.৩ পোস্ট ফরম্যাট অনুযায়ী টেমপ্লেট পার্টস ইউজ করা

index.php ফাইলে while লুপের মধ্যে পোস্ট ফরম্যাট অনুযায়ী টেমপ্লেট পার্ট ফাইল লোড করা হচ্ছে "post-formats" ফোল্ডার থেকে।

```php
get_template_part("post-formats/content", get_post_format());
```
```html
<span class="dashicons dashicons-format-quote"></span>
```

## ৫.৪ - সিঙ্গেল পোস্টে অথর সেকশন দেখানো

পোস্টের অথর বা ইউজারদের প্রোফাইল পিকচার ওয়ার্ডপ্রেস ড্যাসবোর্ড থেকে পরিবর্তন করা যায় না। ওয়ার্ডপ্রেসের একটি সহযোগী ওয়েবসাইট http://en.gravatar.com/ থেকে সাইনআপ করে অথরের প্রোফাইল ইমেজ আপলোড করলে, সেই ইমেজটাই ওয়ার্ডপ্রেসের ইউজার প্রোফাইলে দেখা যায়।

যে কোন অথরের মেটা ইনফরমেশন ফেচ করার জন্য যে ফাংকশনগুলো ব্যবহার করা হয়:

```php
// Get author "Gravatar" image
get_avatar( get_the_author_meta( "ID" ), $size = 96, $default = '', $alt = '', $args = null )
// Get author "Display Name"
echo get_the_author_meta( "display_name" );
```

বিস্তারিত জানতে:
```
https://developer.wordpress.org/reference/functions/get_the_author_meta/
https://developer.wordpress.org/reference/functions/get_avatar/
```

## ৫.৫ - সাইডবার চেক - কোন উইজেট থাকলে শুধু তখনই সাইডবার দেখানো

সিঙ্গেল পোস্ট পেজে উইজেট থাকা বা না থাকার উপর ভিত্তি করে লেআউট পরিবর্তন করতে হলে, single.php ফাইলে নিচের মত করে লজিক লিখতে হবে:

```php
<?php
$alpha_layout_class = "col-md-8";
$alpha_text_class = '';
if( !is_active_sidebar( 'sidebar-1') )
{
	$alpha_layout_class = "col-md-10 offset-md-1";
	$alpha_text_class = "text-center";
}
?>
```

এইচটিএমএল এর মধ্যে যে কোড লিখতে হবে:

```php
<h2 class="post-title <?php echo $alpha_text_class; ?>">
	<?php the_title(); ?>
</h2>
<p<?php echo !empty( $alpha_text_class ) ? ' class="' . $alpha_text_class . '"' : ''; ?>>
	<strong><?php the_author(); ?></strong><br/>
	<?php echo get_the_date(); ?>
</p>
```

নীচের দিকে যেখানে col-md-4 এর মধ্যে উইজেট দেখানো হচ্ছে, সেখানে লিখতে হবে:

```php
<?php
if( is_active_sidebar( 'sidebar-1') ):
?>
<div class="col-md-4">
	<?php
	if(is_active_sidebar( 'sidebar-1' ))
	{
		dynamic_sidebar( 'sidebar-1' );
	}
	?>
</div>
<!--col-md-4-->
<?php
endif;
?>
```

## ৫.৫ - বডি ক্লাস এবং পোস্ট ক্লাস নিয়ে বিস্তারিত

body_class ফাংকশন ব্যবহার করলে body ট্যাগ এ ওয়ার্ডপ্রেস বেশ কিছু class যুক্ত করে দেয়। এই ফাংকশন এ কাস্টম ক্লাস যুক্ত করতে হলে, তা ফাংকশনের প্যারামিটার আকারে যুক্ত করা যায়। যেমন:

```php
<body <?php body_class( array('my-custom-class', 'second-class', 'third-class') ); ?>>
```

**গুরুত্বপূর্ণ**

ওয়ার্ডপ্রেস যে সমস্ত ক্লাসগুলো body_class বা post_class এর মাধ্যমে এ্যাড করে, তার মধ্যে থেকে এক বা একাধিক ক্লাস বাদ দিতে চাইলে functions.php ফাইলে বডি ক্লাস ও পোস্ট ক্লাসের জন্য প্রয়োজনমত add_filter করতে হবে। নিচে কোড স্যাম্পল দেয়া হল:

```php
function alpha_body_class( $classes )
{
	unset( $classes[array_search( "custom_background", $classes )]);
	unset( $classes[array_search( "admin-bar", $classes )]);
	unset( $classes[array_search( "single-format-standard", $classes )]);

	// Video 5.6
	// We can also add my "custom classes" here
	//$classes[] = 'yet-another-class';

	return $classes;
}
add_filter( "body_class", "alpha_body_class" );

function alpha_post_class( $classes )
{
	unset( $classes[array_search( "format-standard", $classes )]);
	unset( $classes[array_search( "status-publish", $classes )]);
	return $classes;
}
add_filter( "post_class", "alpha_post_class" );
```
