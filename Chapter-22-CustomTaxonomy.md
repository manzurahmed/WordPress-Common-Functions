## ২২ - কাস্টম ট্যাক্সোনমি

### ২২.১ - কাস্টম ট্যাক্সোনমি - পরিচিতি, তৈরী করা, ইউআরএল রিরাইট এবং টেমপ্লেট হায়ারার্কি

ট্যাক্সোনমি হচ্ছে যে কোন ধরনের কনটেন্ট গ্রুপ করার একটি ম্যাকানিজম।

ওয়ার্ডপ্রেসে আমরা যে ক্যাটাগরিজ ও ট্যাগস্‌ দেখি, এগুলো ওয়ার্ডপ্রেসের ডিফল্ট ট্যাক্সোনমি।

ক্যাটাগরিজ এবং ট্যাগস এর মধ্যে আমরা যে সকল আলাদা আলাদা ক্যাটাগরি এবং ট্যাগ তৈরী করি, আমরা প্রচলিত ভাষায় এদেরকে ক্যাটাগরি বা ট্যাগ বলে থাকি। সেগুলোকে আসলে একেকটা **টার্ম** বলে।

এর আগের পর্বগুলোতে book নামে কাস্টম পোস্ট টাইপ তৈরী করা হয়েছিল। ধরি, এখানে অনেকগুলো বই এন্ট্রি দেয়া হল। এখন আমরা চাচ্ছি যে, বইগুলোকে বিভিন্ন বিভাগে ভাগ কর, আবার, প্রতিটা বই কোন ভাষায় লেখা হয়েছে, তা এন্ট্রি দিব। যেমন: Travel, Science Fiction, Novel, Adventure, Romance, এ রকম অনেকগুলো বিভাগ রয়েছে। বইয়ের ভাষা হতে পারে, Bengali, English, French, Spanish, Russian, ইত্যাদি।

এ জন্য আমি, দুইটি আলাদা ট্যাক্সোনমি তৈরি করব। একটি বই এর বিভাগের জন্য, অপরটি ভাষার জন্য। এই পর্বে "CPU UI" প্লাগইন দিয়ে কাস্টম ট্যাক্সোনমি তৈরী করা হয়।

**হায়ারারকিক্যাল**

কাস্টম ট্যাক্সোনমি যদি **হায়ারারকিক্যাল** হয়, তবে পোস্টের মধ্যে "Categories" যেভাবে দেখায় সেভাবে কাস্টম ট্যাক্সোনমি দেখাবে। আর যদি **নন-হায়ারারকিক্যাল** হয়, তবে "Tags" এর মত করে দেখাবে।

**কাস্টম ট্যাক্সোনমি তৈরী**

প্রথমে, **language** নামে একটি কাস্টম ট্যাক্সোনমি তৈরী করা হয়। এবার, প্রতিটি বই ওপেন করে তার সাথে Language ট্যাক্সোনমি "English" যুক্ত করে আপডেট করি দিব।

এই অবস্থায়, Books মেনু থেকে Chapters এ গিয়ে English এর উপরে মাউস হোভার করে View সিলেক্ট করলে শুধু English টার্মযুক্ত ট্যাক্সোনমি’র বইগুলো ওয়েবসাইটে দেখাবে, যার URL হবে, http://localhost/alpha/english/। কিন্তু, আমরা যদি চাই যে, এর URL হবে http://localhost/alpha/books/language/english/, তাহলে URL টা অনেক বেশি অর্থবহ হবে।

**URL rewrite**

এখান, URL rewrite করতে হবে। এ জন্য, আমরা CPT UI এর Settings থেকে "Rewrite With Front" কে False করে দিয়ে "Custom Rewrite Slug" এ লিখব, "/books/language"। এরপর "Save Taxonomy" বাটনে ক্লিক করে আমার নতুন সেটিংগুলো সেভ করে নিব।

এবার, Books থেকে Chapter এ যাব। English টার্ম এর উপরে মাউস হোভার করে View লিংকে গেলে শুধু English টার্মযুক্ত বইগুলোর তালিকা দেখাবে। খেয়াল করব যে, এবার URL টি পরিবর্তিত হয়ে http://localhost/alpha/books/language/english/ দেখাচ্ছে।

**কাস্টম ট্যাক্সোনমি ওয়েবসাইটে দেখাতে হবে**

এবার, সিঙ্গেল বুক ভিউতে প্রতিটি বইয়ের জন্য যে ট্যাগ যুক্ত করা হয়েছে, সেগুলো দেখানো পালা। single-book.php ফাইলটি ওপেন করে নিচের কোড লিখি:

```php
the_terms( get_the_ID(), 'language', '', '', '' );
```

কাস্টম ট্যাক্সোনমি দেখানো জন্য **the_terms()** ফাংশনটি ব্যবহৃত হয়, যার প্যারামিটার সিগনেচার হল,

```php
the_terms( $id, $taxonomy, $before = '', $sep = ', ', $after = '' )
```

**ট্যাক্সোনমির জন্য আলাদা পেজ বানানো**

ট্যাক্সোনমির জন্য আলাদা পেজ বানানো যেতে পারে। যেমন: language ট্যাক্সোনমির জন্য বানাতে হবে, taxonomy-language.php ফাইল। আবার, আরও স্পেসিফিক ট্যাক্সোনমি যেমন: english এর জন্য বানাতে হবে, taxonomy-language-english.php ফাইল।

## ২২.২ - কাস্টম ট্যাক্সোনমির টার্মসগুলো বের করা এবং আমরা টার্মস পেজে আছি কিনা সেটা চেক করা

footer.php ফাইলে দু’টা ফিল্টার যুক্ত করা হয়েছে। কোন পেজে আছি, তার উপরে ভিত্তিকে করে কিভাবে কাস্টম ট্যাক্সোনমি বা ট্যাগ দেখানো যায়, সেটা এই পর্বে দেখানো হয়।

```
<?php
// ২২.২ - কাস্টম ট্যাক্সোনমির টার্মসগুলো বের করা এবং আমরা টার্মস পেজে আছি কিনা সেটা চেক করা
$philosophy_footer_tag_heading = apply_filters( "philosophy_footer_tag_heading", __( 'Tags', 'philosophy' ) );
$philosophy_footer_tag_items = apply_filters( "philosophy_footer_tag_items", get_tags() );
?>

<h3><?php echo esc_html( $philosophy_footer_tag_heading ); ?></h3>

<div class="tagcloud">
    <?php
    //the_tags( "", "", "" );
    // ২২.২ - কাস্টম ট্যাক্সোনমির টার্মসগুলো বের করা এবং আমরা টার্মস পেজে আছি কিনা সেটা চেক করা
    if( is_array($philosophy_footer_tag_items) ) {
        foreach( $philosophy_footer_tag_items as $pti) {
            printf( '<a href="%s">%s</a>', get_term_link($pti->term_id), $pti->name );
        }
    }
    ?>
</div> <!-- end tagcloud -->
```

functions.php ফাইলের কনটেন্ট:

```php
function philosophy_footer_language_heading( $title )
{
	if( is_post_type_archive( 'book' ) || is_tax('language') ) {
		
		$title = __( 'Languages', 'philosophy' );
	}

	return $title;
}
add_filter( "philosophy_footer_tag_heading", "philosophy_footer_language_heading" );
// ২২.২ - কাস্টম ট্যাক্সোনমির টার্মসগুলো বের করা এবং আমরা টার্মস পেজে আছি কিনা সেটা চেক করা
function philosophy_footer_language_terms( $tags ) {
	if ( is_post_type_archive('book') || is_tax('language') ) {
		$tags = get_terms(array(
			'taxonomy' => 'language',
			'hide_empty' => true
		));
	}

	return $tags;
}
add_filter( "philosophy_footer_tag_items", "philosophy_footer_language_terms" );
```

## ২২.৩ - ট্যাক্স কোয়েরীর সাথে পরিচয়

এই পর্বে দেখানো হয়েছে কিভাবে WP_Query দিয়ে কাস্টম পোস্টের সাথে যুক্ত করা কাস্টম ট্যাক্সোনমি’র টার্ম দিয়ে ডাটা পুল করা যায়। একে সংক্ষেপে বলা হয়, ট্যাক্স কুয়েরী।

প্রথমে, taxquery.php নামে একটা আলাদা ”পেজ টেমপ্লেট” (Tax Query Example) বানানো হয়। এর সাথে, ওয়ার্ডপ্রেস এ্যাডমিন থেকে একটি নতুন পেজ তৈরী করে, তাতে ঐ পেজ টেমপ্লেটটা সিলেক্ট করে পেজটি পাবলিশ করে দিব।

পেজ টেমপ্লেটে WP_Query দিয়ে কাস্টম পোস্টের ডাটা যেভাবে পুল করা হয়েছে, তা নিচে দেওয়া হল:

```php
<?php
/*
* Template Name: Tax Query Example
*/

$philosophy_query_args = array(
	'post_type' => 'book',
	'posts_per_page' => -1,
	'tax_query' => array(
		array(
			'taxonomy' => 'language',
			'field' => 'slug',
			// For SINGLE tax term
			'terms' => 'english'
			// For MULTIPLE tax term
			//'terms' => array('bangla', 'english')
		)
	)
);

$philosophy_posts = new WP_Query( $philosophy_query_args );

while( $philosophy_posts->have_posts() ) {
	$philosophy_posts->the_post();
	the_title();
	echo "<br />";
}

// Reset to Default Query
wp_reset_query();
```

## ২২.৪ - সিম্পল ট্যাক্স কোয়েরী রিলেশনশিপ

বইয়ের লিস্টের কথাই ধরি। কিছু বই bangla বা english ভাষায় লেখা হয়েছে। কিন্তু, একটি বই bangla এবং english, উভয় ভাষাতেই লেখা হয়েছে। কিন্তু, সমস্যা হল, ট্যাক্স কুয়েরি চালালে ঐ বইটিও পুল হয়ে যাবে, যার দু’টি ভাষা সেট করা আছে।

কিন্তু, আমি চাচ্ছি যে, যে সকল বইয়ের দু’টি ভাষা আছে, সেগুলোই ট্যাক্স কুয়েরিতে আসুক। এই ক্ষেত্রে, ট্যাক্স কুয়েরীতে রিলেশনশিপ স্থাপন করতে হবে। আগের পর্বে লেখা কুয়েরী’র আর্গুমেন্টের tax_query প্যারামিটারে এ্যারে আকারে রিলেশনশিপটা স্থাপন করা হয়েছে।

```php
*/
// ২২.৪ - সিম্পল ট্যাক্স কোয়েরী রিলেশনশিপ
$philosophy_query_args = array(
	'post_type' => 'book',
	'posts_per_page' => -1,
	'tax_query' => array(

		'relation' => 'AND',
		array(
			'taxonomy' => 'language',
			'field' => 'slug',
			'terms' => 'bangla'
		),
		array(
			'taxonomy' => 'language',
			'field' => 'slug',
			'terms' => 'english',
			'operator' => 'NOT IN'
		)

	)
);

$philosophy_posts = new WP_Query( $philosophy_query_args );

while( $philosophy_posts->have_posts() ) {
	$philosophy_posts->the_post();
	the_title();
	echo "<br />";
}

// Reset to Default Query
wp_reset_query();
```

উপরের এই কুয়েরী’র মাধ্যমে **শুধুমাত্র bangla** ল্যাংগুয়েজ টার্ম ব্যবহার করা হয়েছে, এমন বইয়ের লিস্ট দেখাবে।

## ২২.৫ - এক্সটেন্ডেড ট্যাক্স কোয়েরী রিলেশনশিপ এবং নেস্টিং

এই পর্বে "Genre" নামে আরেকটি নতুন কাস্টম ট্যাক্সোনমি তৈরী করা হয়। এবার, "Classic", "Thriller" এবং "Horror" নামে ৩টি নতুন জোনরা টার্ম সেভ করি। এ্যাডমিন প্যানেল থেকে প্রতিটি বইকে ওপেন করে তাদের  প্রত্যেক্যের জন্য একটি করে জোনরা সিলেক্ট করে সেভ করি।

বই এর ডাটাবেজের সিনেরিও হল, কিছু বই বাংলা ও ইংরেজী ভাষায় আছে, কিছু বই শুধু বাংলায়, কিছু বই শুধু ইংরেজীতে। আবার, প্রতিটা বইয়ের আলাদা আলাদা জোনরা যুক্ত করা আছে।

আমার রিকোয়ারমেন্ট হল, এমন সব বই খুঁজে বের করব, যাদের ভাষা english, কিন্তু, bangla নয়, এবং, classic জোনরা’র অন্তর্ভূক্ত।

এর জন্য আমার ট্যাক্স কুয়েরীতে **নেস্টিং কুয়েরী** ব্যবহার করতে হবে এবং কুয়েরীগুলোর মধ্যে **রিলেশনশিপ** ব্যবহার করতে হবে।

আমার কোড ছিল নিম্নরূপ:

```php
$philosophy_query_args = array(
	'post_type' => 'book',
	'posts_per_page' => -1,
	'tax_query' => array(

		'relation' => 'AND',
		array(
			'relation' => 'AND',
			array(
				'taxonomy' => 'language',
				'field' => 'slug',
				'terms' => 'english'
			),
			array(
				'taxonomy' => 'language',
				'field' => 'slug',
				'terms' => 'bangla',
				'operator' => 'NOT IN'
			)
		),
		array(
			'taxonomy' => 'genre',
			'field' => 'slug',
			'terms' => 'classic'
		),
	)
);
```
