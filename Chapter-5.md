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

## ৫.৬ - বডি ক্লাস এবং পোস্ট ক্লাস নিয়ে বিস্তারিত

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

### ৫.৭ - ট্যাগ এবং ক্যাটেগরীর আর্কাইভ

ক্যাটাগরী, ট্যাগ, অথর, সন, মাস বা দিনের পোস্টগুলোকে একটার পর একটা দেখানোর নাম আরকাইভ।

index.php ফাইলকে রিনেম করে category.php এবং tag.php নামে দুইটা ফাইল বানিয়ে, তার মধ্যে প্রয়োজনীয় এডিট করে এই দুইটা পেজের জন্য কাস্টমাইজ করে নিতে হবে।

ক্যাটাগরী আর্কাইভ পেজে যে ক্যাটাগরীর আর্কাইভ দেখাচ্ছে তার টাইটেল দেখানোর জন্য **single_cat_title()** ফাংকশন ও ট্যাগ পেজের টাইটেল দেখানোর জন্য **single_tag_title()** ফাংকশন ব্যবহার করতে হবে।

```php
single_cat_title()
single_tag_title()
```

### ৫.৮ - ডে, মান্থ এবং ইয়ারলি আর্কাইভ

tag.php ফাইলকে কপি করে date.php নামে রিনেম করে নিই।

সন, মাস ও তারিখের টাইটেলের জন্য ওয়ার্ডপ্রেসের ফাংকশন ব্যবহার করে নিরূপণ করে নিতে হবে আমি date এর কোন অবস্থানে রয়েছি, সন এর আর্কাইভে, নাকি, মাসের, নামি, তারিখের আর্কাইভে। if কন্ডিশন দিয়ে এটা করা হয়েছে। কোড নিম্নরূপ:

```php
if( is_month() )
{
    $month = get_query_var( 'monthnum' );
    $dateobj = DateTime::createFromFormat( '!m', $month );
    echo $dateobj->format("F");
}
else if( is_year() )
{
    echo get_query_var( 'year' );
}
else if ( is_day() )
{
    printf("%s/%s/%s", get_query_var('day'), get_query_var('monthnum'), get_query_var('year'));
}
```

### ৫.৯ - get_query_var কে এসকেপ করা

ইউজারের কাছে থেকে যত ধরণের ইনপুট আসবে, সব esc_html বা esc_attr দিয়ে এসকেপ করে নিতে হবে। যেমনঃ

```php
$year = esc_html( get_query_var('day) );
```

### ৫.১০ - অথর পেজ - এবং অথর টেমপ্লেট হায়ারার্কি

অথর এর নাম পেতে **the_author_posts_link()** ফাংকশন ব্যবহার করব। 
অথর এর আইডি বা নাইসনেম দিয়ে যেমনঃ author-1.php বা author-admin.php তৈরী করে অথর পেজের জন্য আলাদা লুক বা ফিল তৈরী করা যায়।

### ৫.১১ - অ্যাটাচমেন্টস প্লাগইনের সাথে পরিচয় এবং এর সাহায্যে চমৎকার একটা স্লাইডার তৈরী করা

Attachments নামে একটা নতুন প্লাগইন ইন্সটল করলে পোস্ট ও পেজে এক বা একাধিক ইমেজকে প্রতিটি পোস্ট বা পেজের সাথে এটাচ বা যুক্ত করা যায়। বাই ডিফল্ট পোস্ট ও পেজে ছবি এটাচ করার অপশন চলে আসে। এছাড়াও ওয়ার্ডপ্রেসে এটাচমেন্টে একটা menu item যুক্ত হয়। এই ডিফল্ট বিহ্যাভিওর পরিবর্তন করা যায়।

এর জন্য প্রথমে থিমে lib নামে একটু ফোল্ডার তৈরী করে তার মধ্যে attachments.php ফাইল তৈরী করার পর ২টা define ভ্যালুর মান পরিবর্তন করে দিতে হবে।

```php
define( 'ATTACHMENTS_SETTINGS_SCREEN', false );
add_filter( 'attachments_default_instance', '__return_false' );
```

এবার এই attachments.php কে functions.php ফাইলে ইনক্লুড করে দিতে হবে।

এছাড়াও, এ্যাটাচমেন্টের জন্য টাইটেল ও ক্যাপশন ফিল্ড থাকে। ক্লাসের টিউটোরিয়ালে শুধু টাইটেল রেখে ক্যাপশনকে বাদ দেয়া হয়েছে। এর জন্য নিচের কোড লিখতে হবে:

```php
function alpha_attachments( $attachments )
{
	$fields = array(
		array(
			'name'		=> 'title',
			'type'		=> 'text',
			'label'		=> __( 'Title', 'alpha' )
		),
	);

	$args = array(
		'label'				=> 'Featured Slider',
		'post_type'			=> array( 'post' ),
		'filetype'			=> array( 'image' ),
		'note'				=> 'Add Slider Images',
		'button_text'		=> __( 'Attach Files', 'alpha' ),
		'fields'			=> $fields,
	);

	$attachments->register( 'slider', $args );
}
add_action( 'attachments_register', 'alpha_attachments' );
```

এই কোডগুলো প্লাগইনের নিজস্ব ডকুমেন্টেশন পেজে পাওয়া যাবে।

বিস্তারিত:
https://github.com/jchristopher/attachments/blob/master/docs/usage.md

যে কোন পোস্টে ৪/৫ টা ইমেজ এ্যাটাচ করে পোস্ট আপডেট করে নিব। এই ইমেজগুলোকে স্লাইডার আকারে পোস্টে দেখানোর জন্য TinySlider নামের একটি স্ক্রিপ্ট ব্যবহার করা হবে। single.php ফাইলে স্লাইডারের HTML মার্ক ও কিভাবে ইন্সটানশিয়েট করতে হবে, তা নীচে দেয়া হল:

```php
if( class_exists('Attachments') )
{
	$attachments = new Attachments( 'slider' );
	if( $attachments->exist())
	{
		while( $attachment = $attachments->get() )
		{ ?>
			<div>
				<?php echo $attachments->image( 'large' ); ?>
			</div>
		<?php
		}
	}
}
```

```js
var slider = tns({
    container: '.slider',
    speed: 300,
    autoplayTimeout: 3000,
    items: 1,
    autoplay: true,
    autoHeight: true,
    controls: false,
    nav: false,
    autoplayButtonOutput: false,
});
```

### ৫.১২ - অ্যাটাচমেন্টস প্লাগইন এর সাহায্যে টেস্টিমোনিয়াল সেকশন তৈরী করা

ক্লাস ৫.১১ এ TinySlider স্ক্রিপ্ট দিয়ে পোস্টের জন্য যে স্লাইডার বানানো হয়েছিল, ঠিক একইভাবে About পেজে টেস্টিমোনিয়াল স্লাইডার বানানো হয়েছে।
About পেজের জন্য আলাদা পেজ টেমপ্লেট বানানো আছে।
lib/attachments.php ফাইলে Page এর জন্য আলাদা করে Testimonial বানানোর জন্য হুক ও কলব্যাক ফাংশন ব্যবহার করা হয়েছে।

```php

function alpha_testimonial_attachments( $attachments )
{
	$fields = array(
		array(
			'name'		=> 'name',
			'type'		=> 'text',
			'label'		=> __( 'Name', 'alpha' )
		),
		array(
			'name'		=> 'position',
			'type'		=> 'text',
			'label'		=> __( 'Position', 'alpha' )
		),
		array(
			'name'		=> 'company',
			'type'		=> 'text',
			'label'		=> __( 'Company', 'alpha' )
		),
		array(
			'name'		=> 'testimonial',
			'type'		=> 'textarea',
			'label'		=> __( 'Testimonial', 'alpha' )
		),
	);

	$args = array(
		'label'				=> 'Testimonials',
		'post_type'			=> array( 'page' ),
		'filetype'			=> array( 'image' ),
		'note'				=> 'Add Testimonial',
		'button_text'		=> __( 'Attach Files', 'alpha' ),
		'fields'			=> $fields,
	);

	$attachments->register( 'testimonials', $args );
}
add_action( 'attachments_register', 'alpha_testimonial_attachments' );

```

### ৫.১৩ - অ্যাটাচমেন্টস প্লাগইন এর সাহায্যে টিম মেম্বার সেকশন তৈরী করা

৫.১২ ক্লাসের টেস্টিমোনিয়ালের মতই টিম সেকশন তৈরী করা হয়েছে।

### ৫.১৪ - এক পেজের অ্যাটাচমেন্টস প্লাগইনের ইনফরমেশন কিভাবে অন্য পেজে নিব

এটাচমেন্ট প্লাগইনের ইন্সটানশিয়েট করার জন্য আর্গুমেন্ট আকারে যে পেজে এ্যাটাচমেন্ট আছে সেই পেজের আইডিকে পাস করা হয়। নিচের কোড স্যাম্পল দ্রস্টব্য।

```php
<?php /* ************** TESTIMONIALS *************** */ ?>
<?php
// Video 5.14
$attachments = new Attachments( 'testimonials', 2 );
if( class_exists('Attachments') && $attachments->exist() ):
?>
<h2 class="text-center">
    <?php _e( 'Testimonials', 'alpha' ); ?>
</h2>
<?php endif; ?>

<div class="testimonials slider text-center">
<?php
if( class_exists('Attachments') )
{
    if( $attachments->exist())
    {
	while( $attachment = $attachments->get() )
	{ ?>
	    <div>
		<?php echo $attachments->image( 'thumbnail' ); ?>
		<h4><?php echo esc_html($attachments->field( 'name' )); ?></h4>
		<p><?php echo esc_html($attachments->field( 'testimonial' )); ?></p>
		<p>
		    <?php echo esc_html($attachments->field( 'position' )); ?>
		    <strong>
		    <?php echo esc_html($attachments->field( 'company' )); ?>
		    </strong>
		</p>
	    </div>
	<?php
	}
    }
}
?>
</div>
```

### ৫.১৫ - থিমে সার্চ ফর্ম যোগ করা

ওয়ার্ডপ্রেস সাইটে এ্যাড্রেস বারে "/s=hello" লিখে এন্টার দিলে সার্চ কাজ করবে।

ওয়ার্ডপ্রেসে ডিফল্ট সার্চ ফর্ম আনতে হলে, নিচের কোড ব্যবহার করব:

```php
echo get_search_form();
// OR
get_search_form(true);
```

### ৫.১৬ - সার্চ ফর্মের কোড পরিবর্তন করা

get_search_form ফাংশনটি থিমের **searchform.php** নামের ফাইল থেকে সার্চ ফর্ম দেখাবে। যদি এ রকম ফাইল না থাকে, তবে **ডিফল্ট** সার্চ ফর্ম দেখাবে।

সার্চের HTML5 কমপ্লায়েন্ট মার্কআপ পেতে functions.php ফাইলে থিমের জন্য html5 সাপোর্ট নিতে হবে। সেজন্য নিচের কোড লিখতে হবে:

```
function wpdocs_after_setup_theme() {
    add_theme_support( 'html5', array( 'search-form' ) );
}
```
এরপর ‍searchform.php ফাইলে সার্চ ফর্মের মার্কআপ লিখব। নীচে একটি মার্কআপ দেয়া হয়েছে। তবে, এই মার্কআপ নিজের প্রয়োজনমত পরিবর্তন করে নেয়া যাবে।

```html
<form role="search" method="get" class="search-form" action="<?php echo home_url( '/' ); ?>">
    <label>
        <span class="screen-reader-text"><?php echo _x( 'What do you want to search:', 'alpha' ) ?></span>
        <input type="search" class="search-field"
            placeholder="<?php echo esc_attr_x( 'Search …', 'placeholder' ) ?>"
            value="<?php echo get_search_query() ?>" name="s"
            title="<?php echo esc_attr_x( 'Search for:', 'alpha' ) ?>" />
    </label>
    <input type="submit" class="search-submit"
        value="<?php echo esc_attr_x( 'Search', 'submit button' ) ?>" />
</form>
```
