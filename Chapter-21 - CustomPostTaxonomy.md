## ২১.১ কাস্টম পোস্ট - কি এবং কেন?

ওয়ার্ডপ্রেসে ৫ রকমের পোস্ট টাইপ (post_type) আছে:

1. post
2. page
3. pttachment
4. nav_menu_item
5. revision

ওয়ার্ডপ্রেসে রেগুলার কনটেন্ট নিয়ে কাজ করার জন্য Posts সেকশন রয়েছে। কিন্তু, যদি মুভির ডাটাবেজ বানাতে চাই, তবে, কাস্টম ফিল্ড দিয়ে এটা বানানো সম্ভব। কিন্তু, এই কাস্টম ফিল্ডগুলো প্রত্যেক পোস্টের সাথেই লোড হবে। প্রোগ্রামার ছাড়া অন্যান্য কোন নরমাল এ্যাডমিন ইউজার সেগুলো দেখে স্বাচ্ছন্দ বোধ নাও করতে পারেন। কোড লিখে হয়ত এগুলো নরমাল ইউজারদের কাছে থেকে হাইড করে ফেলা যাবে।

এই প্যারা থেকে মুক্তি দিতে ওয়ার্ডপ্রেসে আছে, কাস্টম পোস্ট টাইপ। কাস্টম পোস্ট টাইপ ব্যবহারে ইউআরএল এর স্ট্রাকচার পরিবর্তন করা সহজ হয়। যেমন: ক্যাটাগরীর ক্ষেত্রে ইউআরএল হয়, http://www.example.com/category/history। কিন্তু, মুভি ডাটাবেজের জন্য প্রয়োজন হবে http://www.example.com/movies, বা, http://www.example.com/movie/2017, বা, http://www.example.com/movie/2017/movie-name-here, ইত্যাদি।

### ট্যাক্সোনমি ও টার্মস

ওয়ার্ডপ্রেসের category, tag - এগুলো taxonomy’র type।

Grouping, soring এর জন্য ট্যাক্সোনমি ব্যবহার করা হয়।

ট্যাক্সোনমি’র মধ্যে যে শব্দগুলো ব্যবহার করা হয়, তাদেরকে terms বলে।


## ২১.২ - কাস্টম পোস্ট তৈরী করার জন্য সিপিটি ইউআই প্লাগইন ব্যবহার করা

register_post_type ফাংশন দিয়ে কাস্টম পোস্ট টাইপ তৈরী করা হয়। এটি init হুকের মধ্যে ব্যবহার করতে হয়।

Webdev Studios’র তৈরী "Custom Post Type UI" প্লাগইন দিয়ে কাস্টম পোস্ট তৈরীর সকল কাজ করা যায়। এই টিউটোরিয়ালে কাস্টম পোস্ট তৈরীর জন্য এই প্লাগইনটি ব্যবহার করা হয়েছে।

পোস্ট টাইপ **সবসময় সিংগুলার** হবে।

এর সাথে Plural Label এবং Singular Label বলে দিতে হবে।

কাস্টম পোস্ট টাইপ তৈরী করার মেনু থেকে ২/৩ টা বই এর ডামি ডাটা সেভ করি। এই বইগুলো এখনও কোন পেজ এ কাস্টম কুয়েরি দিয়ে দেখানো হয়নি। তবে address bar এ গিয়ে http://localhost/alpha/book এভাবে url লিখলে, আমি বইগুলোর লিস্ট দেখতে পাব।

```php
http://localhost/alpha/book/
```


## ২১.৩ - কাস্ট পোস্টের ডিটেইল সেটিংস (অত্যন্ত গুরুত্বপূর্ণ)

এই ভিডিওতে Custom Post Type UI প্লাগইনের সেটিং ব্যবহার করে কিভাবে কাস্টম পোস্ট টাইপের বিভিন্ন সেটিং, যেটা প্রোগ্রাম্যাটিক্যালি করতে হয়, পরিবর্তন করে কাস্টম পোস্টের বৈশিষ্ট্য পরিবর্তন করা যায়, সে সম্পর্কে আলোচনা করা হয়েছে।


## ২১.৪ - কাস্টম পোস্ট আমাদের নিজেদের প্লাগইন থেকে তৈরী করা

এর আগের ক্লাসে Custom Post Type UI প্লাগইন দিয়ে যে কাস্টম পোস্ট টাইপ রেজিস্টার করা হয়েছিল, ঐ প্লাগইন থেকে Get Code ব্যবহার করে সিপিটি’র কোড জেনারেট করে নিতে হবে। মেনু থেকে CPT UI এ ক্লিক করে View Post Types ট্যাবে ক্লিক করতে হবে। এর বাম দিয়ে Get Code লিংকটা পাওয়া যাবে।

থিমের Plugins ফোল্ডারে philosophy-companion নামে একটি ফোল্ডারে মধ্যে থিমের জন্য একটি প্লাগইন বানাব এবার। ফোল্ডারের নামের সাথে মিল রেখে একই নামে একটি ফাইল বানাব (philosophy-companion.php)। ফাইলটি ওপেন করে তার মধ্যে প্লাগইনের ডিক্লারেশন ব্লক লিখতে হবে।

```php
<?php
/*
Plugin Name: Philosophy Companion
Plugin URI:
Description: Companion plugin for the philosophy theme
Version: 1.0
Author: LWHH
Author URI:
License: GPLv2 or later
Text Domain: philosophy-companion
*/
```

এর নীচে CPT UI এর জেনারেট করা কোড কপি করে নিয়ে এসে পেস্ট করে দিতে হবে। এরপর, এ্যাডমিন থেকে Plugins এ গিয়ে Custom Post Type UI প্লাগইনটি ডিএ্যাকটিভেট করে দিব।

একই সাথে, যদি আমরা Plugins এর লিস্টের দিকে তাকাই, তবে দেখতে পাব, আমার Philosophy Companion নামের প্লাগইনটি দেখা যাচ্ছে। প্লাগইনটি এ্যাকটিভ করি। এ্যাকটিভ করার সাথে সাথে Books নামের সিপিটি’টি রেজিস্টার হয়ে যাবে এবং বামের মেনুতে Books নামের নতুন একটি মেনু যুক্ত হবে।

যে কাজটি থার্ডপার্টি প্লাগইন দিয়ে এতক্ষণ করা হচ্ছিল, সেটা ওয়ার্ডপ্রেসের ন্যাটিভ ফাংশন দিয়ে করে ফেললাম। আর, এর মধ্যে দিয়ে থিমের জন্য নতুন একটি প্লাগইনও তৈরী করা শিখে ফেললাম।

পরবর্তীতে থিম ডিসট্রিবিউট করার সময় থিমের মধ্যে plugins নামে একটি ফোল্ডার বানিয়ে আমাদের নতুন প্লাগইনকে .zip করে রেখে দিতে হবে। এবং, TGMPA দিয়ে রিকোয়ার্ড প্লাগইন হিসাবে ইন্সটল করার ব্যবস্থা করে দিতে হবে।

## ২১.৫ - কোড জেনারেশন টুলের সাহায্য নেয়া

অনলাইন দু’টি ওয়েবসাইটের মাধ্যমে ওয়ার্ডপ্রেসের এ্যাডমিন ও ওয়েবসাইটের ফিচারের জন্যবিভিন্ন ধরনের কোড জেনারেটর করা যায়। যেমন: widget, menu, custom post type, admin option, nav menu, sidebar।

সাইট দু’টো হল:

1. WordPress Code Generator 1: https://www.wp-hasty.com/tools/
2. WordPress Code Generator 2: https://generatewp.com/

#### লক্ষ্যনীয়:

Custom Post Type UI এর কোড অনেক পরিষ্কার ও সংক্ষেপিত।

## ২১.৬ - কাস্টম পোস্টের টেমপ্লেট হায়ারার্কি বোঝা এবং পার্মালিংক ফ্লাশ করা।

### পার্মালিংক ফ্লাশ

কোন কারণে পার্মালিংকের পরিবর্তন ঘটানো হলে Settings -> Permalinks মেনুতে গিয়ে Save Changes বাটনে ক্লিক করতে হবে। এতে পার্মালিংকের নতুন পরিবর্তনকে ওয়ার্ডপ্রেস কার্যকর করে। এই প্রক্রিয়াকে **Permalink flash** করা হবে।

আমরা কাস্টম পোস্ট তৈরী করেছিলাম, তার url ছিল, http://localhost/alpha/book। এখন আমরা যদি চাইতাম, book নয়, আমার সব বইয়ের লিস্ট দেখানোর জন্য পার্মালিংকে books দেখাবে এবং সিঙ্গেল বইয়ের ক্ষেত্রে book দেখাবে।

কাস্টম পোস্টের পার্মালিংক স্ট্রাকচারে পরিবর্তন করতে হলে, has_archive আর্গমেন্ট ব্যবহার করা হয়। এটির ডিফল্ট মান থাকে true। পার্মালিংকে আমাদের ডিজায়ার্ড পরিবর্তনের জন্য এই true এর পরিবর্তনে লিখতে হবে, "books"। এবং $args আর্গুমেন্টে নতুন একটি প্যারামিটার পাস করতে হবে:

```php
'rewrite' => array(
	'with_front' => false
)
```

এরপর, **পার্মালিংক ফ্ল্যাশ** করতে হবে।

আমরা এখনও কাস্টম পোস্টের পেজিনেশন লিখি নাই। কিন্তু, ফলব্যাক হিসাবে index.php এর পেজিনেশন লিখা হয়েছিল, সেটাই কাজ করছিল।

এই কাস্টম পোস্ট টাইপের জন্য আলাদা করে কোন স্টাইলিং দিতে চাইলে (যেমন একটা হেড লাইন যুক্ত করা), আমাদের সিপিটি’র slug (অর্থাৎ, "book") দিয়ে একটি ফাইল তৈরী করতে হবে, যার নাম হবে  archive-book.php। এরমধ্যে index.php থেকে সব কোড কপি করে এই archive-book.php ফাইলে পেস্ট করতে হবে। এর মধ্যে প্রয়োজনীয় পরিবর্তন করে নিতে হবে।

সিপিটি’র সিঙ্গেল এন্ট্রিকে দেখানোর জন্য single-book.php ফাইল তৈরী করে তার মধ্যে single.php এর কনটেন্ট কপি করে পেস্ট করতে হবে। এরপর তার মধ্যে প্রয়োজনীয় পরিবর্তনগুলো করতে হবে।

## ২১.৭ - কাস্টম পোস্টে কন্ডিশনালি সার্চ করা

আমাদের ওয়েবসাইটে রেগুলার পোস্টের পাশাপাশি এবং কাস্টম পোস্টও রয়েছে। এখন একটি সিনোরিও চিন্তা করি। ধরে হোম পেজ থেকে সার্চ দিলে শুধু রেগুলার পোস্টের মধ্যে সার্চ করবে। কিন্তু, পিসিটি পেজে গিয়ে কেউ যদি সার্চ করে, তবে সিপিটি’র মধ্যে সার্চ করে দেখাবে।

খুবই লজিক্যাল একটি রিকোয়ারমেন্ট।

functions.php ফাইলে সার্চ ফর্ম দেখানোর জন্য আমরা philosophy_search_form ফাংশন ব্যবহার করেছিলাম। এখানে ফর্মের মধ্যে একটি হিডেন ইনপুট ফিল্ড conditionally যুক্ত করব।

```php
$post_type = <<<PT
<input type="hidden" name="post_type" value="post">
PT;

// এখন চেক করা হবে, বর্তমানে আমরা কাস্টম পোস্ট টাইপের আর্কাইভ পেজে আছি কিনা।
// যদি থাকি, তবে, post_type এর ভ্যালু পরিবর্তন করে "book" করে দিব
if ( is_post_type_archive( 'book' ) ) {
	$post_type = <<<PT
<input type="hidden" name="post_type" value="book">
PT;
	}
```

## ২১.৮ - কাস্টম পোস্টে মেটাবক্স বাইন্ড করা এবং কাস্টম মেটা কোয়েরী চালানো

এই পর্বে কাস্টম পোস্টের সাথের মেটাবক্স যুক্ত করা হয়েছে এবং মেটা ডাটার উপরে ভিত্তি করে কাস্টম কোয়েরী চালান হয়েছে।

প্রথমে একটি টেমপ্লেট ফাইল বানিয়ে নেই। index.php ফাইলকে কপি-পেস্ট করে featured-book.php নামে একটি নতুন ফাইল তৈরী করে নিব। সবার উপরে লিখব:

```php
/*
Template Name: Featured Books
*/
```

এবার ACF, CMB2 বা CodeStar দিয়ে একটি কাস্টম মেটাফিল্ড বানিয়ে book সিপিটির মধ্যে ৪/৫ টি বইয়ের ইন্ট্রি দিই। এন্ট্রি দেয়ার সময় is_featured চেকবক্স অন করে দিতে হবে।

এবার, featured-book.php পেজে একটি কাস্টম কুয়েরী লিখব, যেটি book নামের সিপিটি থেকে থেকে is_featured যুক্ত বইগুলো ডাটাবেস থেকে পুল করে আনবে।

```php
$philosophy_cpt_arguments = array(
                    'post_type' => 'book',
                    'meta_key' => 'is_featured',
                    'meta_value' => true
                );

                $philosophy_books = new WP_Query( $philosophy_cpt_arguments );

                while( $philosophy_books->have_posts() ) {
                    $philosophy_books->the_post();

                    get_template_part( "template-parts/post-formats/post", get_post_format() );
                }
```

## ২১.৯ - কাস্টম পোস্ট কিভাবে আপনাকে চমৎকার চমৎকার ওয়ার্ডপ্রেস থিম তৈরী করতে সাহায্য করবে?

এই পর্বে থিম ফরেস্টের কিছু টপ অথরের বেস্ট সেলিং থিম দেখানো হয়েছে। এই থিমগুলোর সাফল্যে কারণগুলো বলা হয়েছে।

থিমগুলো সব সেকশন বেজড। প্রতিটি সেকশনের প্রেজেন্টেশন খেয়াল করলে দেখা যায় যে, এখানে সঠিক টাইপোগ্রাফির ব্যবহার, হেডলাইন, সাব-হেডলাইনের মধ্যে কালার প্যালেটের সুন্দর মিল রাখা হয়েছে, সাথে ম্যাচিং গ্রাফিক ইমেজ ব্যবহার করা হয়েছে।

আর, সেকশনগুলো পেজ বিল্ডার (যেমন:, ভিজুয়াল কম্পোজার, কিং কম্পোজার, এলিমেন্টর) দিয়ে তৈরী করার একটা স্ট্রং প্র্যাকটিশ চলছে এখন। 

একটা থিমের সাফল্যের পিছনে শুধু প্রোগ্রামিংই নয়, বরং, একটা টিম ওয়ার্ক কাজ করেছে। কেউ প্রোগ্রামিং দেখেছেন, কেউ সুন্দর গ্রাফিক ডিজাইন করেছেন, কেউ UI/UX দেখেছেন। একটা ভাল কাজের পিছনে টিমওয়ার্ক রয়েছে। একা কখনও এত সুন্দর প্রজেক্ট সম্পন্ন করা যায় না্

থিমের সেকশনগুলো খেয়াল করলে দেখা যায় যে, একগুলো বেশির ভাগই কাস্টম পোস্ট দিয়ে করা যায়। সুতরাং, কাস্টম পোস্ট নিয়ে ব্যাপক পড়াশুনা ও প্র্যাকটিস করার উপরে সাফল্য নির্ভর করবে।

## ২১.১০ - দু্ই ধরনের কাস্টম পোস্টের মাঝে প্যারেন্ট চাইল্ড রিলেশনশিপ এবং ইউআরএল রিরাইট

এই ভিডিওতে দুই বা ততোধিক কাস্টম পোস্ট টাইপের মধ্যে রিলেশনশিপ তৈরীর পদ্ধতি নিয়ে আলোচনা করা হয়েছে। সেই সাথে, url rewriting টাই দেখানো হয়েছে। উদাহরণ হিসাবে book এবং chapter এর মধ্যে রিলেশনশিপের ব্যাপারটা তুলে ধরা হয়েছে। ওয়েবসাইটে যখন ডাটা দেখাবে, তখন url হবে এরকম, localhost/philosophy/book/chapter-1।

প্যারেন্ট ও চাইল্ডৈর মধ্যে রিলেশন স্টাবলিস করার পর URL Rewrite করা হয়েছে। URL Rewrite করার জন্য post_type_link হুক ব্যবহার করা হয়েছে, যার কলব্যাক ফাংশনটি নিচের মত ছিল:

function philosophy_cpt_slug_fix( $post_link, $id ) {
	$p = get_post( $id );

	if( is_object( $p ) && 'chapter' == get_post_type( $id )) {
		$parent_post_id = get_field( 'parent_book' );
		$parent_post = get_post( $parent_post_id );

		if( $parent_post ) {
			$post_link = str_replace( '%book%', $parent_post->post_name, $post_link );
		}

		return $post_link;
	}
}
add_filter( 'post_type_link', 'philosophy_cpt_slug_fix', 1, 2 );

**post_type_link** হুকটি সবসময় slug তৈরীর কাজে ব্যবহৃত হয়।

## ২১.১১ - প্যারেন্ট কাস্টম পোস্ট থেকে মেটা কোয়েরী দিয়ে চিলড্রেন কাস্টম পোস্ট খুঁজে বের করা

আগের ভিডিওতে book এবং chapter এর মধ্যে রিলেশনশিপ তৈরী করা হয়েছে। একটা বই এর অধীনে অনেকগুলো চ্যাপ্টার থাকতে পারে। ধরি, ওয়েবসাইটে আমরা Book 1 নামে বইটি দেখছি। বইয়ের সিঙ্গেল ভিউ পেজে বইটির ডিটেইলের নীচে এর চ্যাপ্টারগুলোর লিস্ট দেখানো হবে। এর জন্য single-book.php ফাইলে কিছু WP_Query দিয়ে মেটা কুয়েরী চালানো হবে এবং সেগুলো আউটপুট আকারে দেখানো হবে।

```php
$philosophy_ch_args = array(
    'post_type' => 'chapter',
    'posts_per_page' => -1,
    'meta_key' => 'parent_book',
    'meta_value' => get_the_ID()
);

$philosophy_chapters = new WP_Query( $philosophy_ch_args );
//echo $philosophy_chapters->found_posts;
echo '<h3>';
_e( "Chapters", "philosophy" );
echo '</h3>';

while( $philosophy_chapters->have_posts() ) {
    $philosophy_chapters->the_post();
    $philosophy_chl = get_the_permalink();
    $philosophy_cht = get_the_title();

    printf( "<a href='%s'>%s</a><br />", $philosophy_chl, $philosophy_cht );
}
```

##২১.১২ - অ্যাডমিন প্যানেলে কাস্টম পোস্ট রিলেশনশিপের ভিজ্যুয়াল রিপ্রেজেন্টেশন এবং ড্র‍্যাগ/ড্রপের মাধ্যমে অর্ডার পরিবর্তন করা

"CMB2 Attached Posts" plugin এর .zip ফাইল ডাউনলোড করে ইন্সটল করে নিতে হবে। cmb2-attached-posts.php ফাইলটি inc ফোল্ডারে রেখে functions.php ফাইলে require_once দিয়ে ইনক্লুড করে নিতে হবে। ফাইলটি ওপেন করে নিচের দুটি লাইন মুছে বা কমেন্ট করে দিতে হবে।

```php
//require_once WPMU_PLUGIN_DIR . '/cmb2/init.php';
//require_once WPMU_PLUGIN_DIR . '/cmb2-attached-posts/cmb2-attached-posts-field.php';
```

এখন cmb2_attached_posts_field_metaboxes_example ফাংশনে কারেন্ট book এর আইডি দিয়ে এর সাথে রিলেটেড চ্যাপ্টারগুলো বের করতে হবে। যেহেতু আমরা যখন একটি বইয়ের চ্যাপ্টারের অর্ডার পরিবর্তন করব, তখন আমরা এ্যাডমিন প্যানেলে থাকব। অ্যাডমিন প্যানেলে get_the_ID() ফাংশন দিয়ে কারেন্ট বইয়ের আইডি পাওয়া যাবে না। তাই বিকল্প উপায়ে বইয়ের আইডি বের করতে হবে।

```php
// Video 21.12
// We are working in Admin panel.
// So, we need to find out current book's ID.
$post_id = 0;
if( isset( $_REQUEST['post'] ) || isset( $_REQUEST['post_ID'] ) ) {
	$post_id = empty( $_REQUEST['post_ID'] ) ? $_REQUEST['post'] : $_REQUEST['post_ID'];
}

$example_meta = new_cmb2_box( array(
	'id'           => 'cmb2_attached_posts_field',
	'title'        => __( 'Attached Posts', 'philosophy' ),
	'object_types' => array( 'book' ), // Post type
	'context'      => 'normal',
	'priority'     => 'high',
	'show_names'   => false, // Show field names on the left
) );

$example_meta->add_field( array(
	'name'    => __( 'Chapters', 'philosophy' ),
	'desc'    => __( 'Drag posts from the left column to the right column to attach them to this page.<br />You may rearrange the order of the posts in the right column by dragging and dropping.', 'philosophy' ),
	'id'      => 'attached_cmb2_attached_posts',
	'type'    => 'custom_attached_posts',
	'column'  => true, // Output in the admin post-listing as a custom column. https://github.com/CMB2/CMB2/wiki/Field-Parameters#column
	'options' => array(
		'show_thumbnails' => true, // Show thumbnails on the left
		'filter_boxes'    => true, // Show a text box for filtering the results
		'query_args'      => array(
			'posts_per_page' => -1,
			'post_type'      => 'chapter',
			// Video 21.12
			'meta_key'		=> 'parent_book',
			'meta_value'	=> $post_id
		), // override the get_posts args
	),
) );
```

"CMB2 Attached Posts" plugin ব্যবহার করা হলে একটি Book এর মধ্যে তার রিলেটেড চ্যাপ্টারগুলোকে "CMB2 Attached Posts" এর নিজস্ব ড্রাগ-এন্ড-ড্রপ অর্ডার ম্যানেজমেন্ট এরিয়ার মধ্যে দেখা যাবে। এখানে ইচ্ছামত রিলেটেড চ্যাপ্টারগুলোর অর্ডার পরিবর্তন করা যাবে।

আমাদের সেট করা অর্ডার অনুযায়ী ফ্রন্টএন্ডে একটি বইয়ের চ্যাপ্টারগুলোকে দেখাতে হলে নিচের কোড প্রয়োজন হবে। এই কোড single-book.php ফাইলে লিখতে হবে।

```php
$philosophy_cmb2_chapters = get_post_meta( get_the_ID(), 'attached_cmb2_attached_posts', true );
//print_r($philosophy_cmb2_chapters);

foreach( $philosophy_cmb2_chapters as $pchapter) {
    $philosophy_chl = get_the_permalink( $pchapter);
    $philosophy_cht = get_the_title($pchapter);

    printf( "<a href='%s'>%s</a><br />", $philosophy_chl, $philosophy_cht );
}
```
