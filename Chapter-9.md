### ৯.১ - কাস্টম কোয়েরীর সাথে পরিচয়

একটা পেজে নির্দিষ্ট কিছু পোস্ট, যেমন:

- কোন একটা ট্যাগের লেটেস্ট পোস্টসমূহ দেখাতে,
- অথবা, সব ক্যাটাগরীর মধ্যে ২/৩টা ক্যাটাগরীর পোস্ট দেখাতে,
- অথবা, একাধিক অথরের মধ্যে প্রতিটি অথরের সর্বশেষ ৩টা পোস্ট দেখাতে

এ রকম অনেক কম্বিনেশন করে পোস্ট দেখাতে কাস্টম কোয়ারী ব্যবহার করা হয়ে থাকে।

### ৯.২ - কাস্টম কোয়েরী রান করা - Using setup_postdata()

অনেকগুলো উপায়ে কাস্টম কুয়েরী রান করা যায়। প্রথমে **get_posts()** ফাংশনটির ব্যবহার দেখানো হয়েছে।

```php
$_p = get_posts(
   array(
      'posts_per_page' => 2,
      'post__in' => array(40,42,46,31,38),
      'orderby' => 'post__in'
   )
);
```

বিস্তারিত পড়তে: https://codex.wordpress.org/Template_Tags/get_posts

get_posts() ফাংশন ব্যবহারের পর আর while লুপ ব্যবহার করা যাবে না। তার পরিবর্তে foreach লুপ চালাতে হবে। লুপের প্রথমেই setup_postdata() ফাংশন ব্যবহার করে কারেন্ট পোস্টকে কনটেক্সে আনতে হবে। তাহলে পোস্টের সাথে রিলেটেড ফাংশনগুলো অবিকল ব্যবহার করা যাবে। যেমন: get_the_ID(), the_ID(), the_post(), the_permalink(), ইত্যাদি।

**NOTE:** each( $_p as $post ) লুপের মধ্যে ভ্যারিয়েবলের এর নাম অবশ্যই $post রাখতে হবে।

কাস্টম কুয়েরী রান করার পরে foreach লুপের পর অবশ্যই wp_reset_postdata() ফাংশনটি ব্যবহার করতে হবে।

```php
wp_reset_postdata();
```

### ৯.৩ - কাস্টম কোয়েরীর পেজিনেশন

কাস্টম কুয়েরী পেজিনেশনের জন্য যে স্থানে পেজিনেশন দেখাবো, সেখানে নীচে কোড লিখতে হবে:

```php
echo paginate_links(
    array(
        'total' => ceil( count( $post_ids ) / $posts_per_page )
    )
);
```

কাস্টম কুয়েরীতে পেজিনেশনকে কাজ করানোর জন্য ২টা আর্গুমেন্ট পাস করতে হবে, $posts_per_page এবং $paged। কাস্টম কুয়েরীতে যতগুলো পোস্ট পুল করা হচ্ছে তাকে $posts_per_page দিয়ে ভাগ করে পেজিনেশনের পেজের লিংকগুলোকে ডিনামিক করা হচ্ছে।

Technically, the function can be used to create paginated link list for any area.

WP Ref Link: https://developer.wordpress.org/reference/functions/paginate_links/

```php
paginate_links( string|array $args = '' )
```
অনেকগুো আর্গুমেন্ট রয়েছে। কোডেক্স থেকে আর্গুমেন্টগুলো সম্পর্কে বিস্তারিত জানা যাবে।

### ৯.৪ - কাস্টম কোয়েরী তে setup_postdata() ইউজ না করে কিভাবে পোস্ট দেখানো যায়

setup_postdata() ফাংশন ব্যবহার করে পোস্টের বিভিন্ন এট্রিবিউট ব্যবহার করে কাস্টম পোস্টের লিস্ট দেখানো হচ্ছে। **এইটাই আইডিয়াল পদ্ধতি।**

এছাড়াও, **অন্যভাবে লিস্ট** দেখানো যায়।

foreach লুপের মধ্যে $post এ্যারে ভ্যারিয়েবলকে print_r দিয়ে ইন্সপেক্ট করে এর মধ্যে থাকা এট্রিবিউটগুলোকে দেখে নিতে হবে। এরপর এই এট্রিবিউটগুলো echo করে লিস্ট বানানো যাবে। যেমন:

```php
echo apply_filters( 'the_title', $post->post_title ); // -- Post title
echo esc_rul( $post->guid ); // - Post permalink
```

### ৯.৫ - কাস্টম কোয়েরীতে WP_Query ক্লাসের ব্যবহার 

WP_Query ক্লাস ব্যবহার ওয়ার্ডপ্রেসের কাস্টম কুয়েরীর একটি বিশাল সাম্রাজ্যে খুব ইফিশেয়েন্টলি কাজ করা যায়। নীচের লিংক থেকে WP_Query সম্পর্কে বিস্তারিত জানা যাবে।

https://codex.wordpress.org/Class_Reference/WP_Query

WP_Query ব্যবহার করে পোস্টের যে ডাটাসেট এ্যারে পাওয়া যায় যায়, তা have_posts ব্যবহার করে while লুপ দিয়ে ইটারেট করে প্রতিটি পোস্টের বিভিন্ন এ্যাট্রিবিউটকে ওয়েবসাইটে দেখানো যায়। ফলে পোস্ট রিলেটেড ওয়ার্ডপ্রেসের স্ট্যান্ডার্ড সকলগুলো ফাংশনই ব্যবহার করার যায়। যেমন, have_posts, the_post, the_title, the_ID, ইত্যাদি।

```php
$paged = get_query_var("paged") ? get_query_var("paged") : 1;
$posts_per_page = 2;
$total = 9;
$post_ids = array(31,38,40,42,44,45,46);

$_p = new WP_Query(
  array(
      'posts_per_page' => $posts_per_page,
      'post__in' => $post_ids,
      'numberposts' => $total,
      'orderby' => 'post__in',
      "paged" => $paged
  )
);
 ```

**নোট:** WP_Query ব্যবহার করে পাওয়া ডাটা লুপের মাধ্যমে দেখানো শেষ হওয়ার পরপরই ****অবশ্যই**** wp_reset_query() ব্যবহার করতে হবে।

### ৯.৬ - WP_Query ক্লাসের পেজিনেশন

পেজিনেশন দেখানোর জন্য নিচের কোড ব্যবহার করতে হবে:

```php
<?php
 echo paginate_links(
     array(
         'total' => $_p->max_num_pages, // In-built property. WP calculates it automatically
         'current' => $paged, // need to calculate seperately
         'prev_text' => __( 'New Posts', 'alpha'),
         'next_text' => __( 'Old Posts', 'alpha'),
         //'prev_next' => true, // When 'true', NO need to specify this argument
     )
 );
 ?>
```

অথবা, শুধু একটি ক্যাটাগরি তুলে আনার জন্য

```php
$_p = new WP_Query(
     array(
         // 'category_name' => 'uncategorized', // Pass here "Category Slug"
         'tag' => 'special'
         'posts_per_page' => 3,
         'paged' => $paged
     )
 );
```

### ৯.৭ - WP_Query ক্লাসের রিলেশনশিপ এবং জয়েনিং

আগের ইপিসোডে একটি ক্যাটাগরীর পোস্টগুলো তুলে আনা হয়েছিল। যদি এর সাথে অন্য কোন ক্রাইটেরিয়ার পোস্ট, যেমন, নির্দিষ্ট কোন ট্যাগ, বা তারিখের পোস্ট, ইত্যাদি, তুলে আনতে হলে রিলেশন ও জয়েন ব্যবহার করতে হবে।

মনে করি, এমন সিচুয়েশন আসল যে, আমাকে uncategorized ক্যাটাগরির সব পোস্ট এবং ঐ সমস্ত পোস্ট পুল করতে হবে যাদের ট্যাগ 'special'। এ রূপ ক্ষেত্রে tax_query করতে হবে। tax_query তে আমরা relation এর ক্ষেত্রে OR ব্যবহার করব এবং দু’টি কুয়েরী ক্লজকে join করব।

```php
$_p = new WP_Query(
  array(
      // 'category_name' => 'uncategorized', // Pass here "Category Slug"
      // 'tag' => 'special',
      'posts_per_page' => 2,
      'paged' => $paged,
      'tax_query' => array(
          'relation' => 'OR',
          array(
              'taxonomy' => 'category',
              'field' => 'slug',
              'terms' => array('uncategorized')
          ),
          array(
              'taxonomy' => 'post_tag',
              'field' => 'slug',
              'terms' => array('special')
          )
      )
  )
);
```

### ৯.৮ - WP_Query তে ডেট এবং পোস্ট স্ট্যাটাস দিয়ে সার্চ করা

WP_Query তে আরও অনেক ধরণের আর্গুমেন্ট পাস করা যায়। যেমন: কোন মাস ও বছর দিয়ে পোস্ট পুল করার জন্য, অথবা, পোস্ট এর post_status দিয়ে যেমন: publish, draft, future, ইত্যাদি দিয়ে কুয়েরী করে পোস্ট ডাটা পুল করা যায়।

#### মাস ও বছর দিয়ে সার্চ করার জন্য:

```php
'monthnum' => 5,
                        'year' => 2018,
```

#### post_status দিয়ে সার্চ করার জন্য:

```php
'post_status' => 'publish'
```

### ৯.৯ - WP_Query তে পোস্ট ফরম্যাট দিয়ে সার্চ করা

এমন একটা সিনেরিয়ো চিন্তা করি, যেখানে একটি পেজে শুধু অডিও ও ভিডিও ফরম্যাট এর পোস্টগুলোকে পুল করে ঐ পেজে দেখাবো। সে ক্ষেত্রে WP_Query ক্লাস ব্যবহার করে তাতে tax_query তে এ্যারে আর্গুমেন্টে নীচের মত করে কোড দিতে হবে:

```php
'tax_query' => array(
 'relation' => 'OR',
 array(
     'taxonomy' => 'post_format',
     'field' => 'slug',
     'terms' => array(
         'post-format-audio',
         'post-format-video',
      ),
      'operator' => 'NOT IN'
    ),
)
```

যদি এমন প্রয়োজন হত যে, অডিও ও ভিডিও পোস্ট ফরম্যাট বাদে বাকি সব পোস্ট পুল করতে হবে, সে ক্ষেত্রে শুধু **"operator"** আর্গুমেন্ট পাস করলেই হবে। যেমন:

```php
'operator' => 'NOT IN'
```

### ৯.১০ - WP_Query তে মেটা কি এবং মেটা ভ্যালু দিয়ে সার্চ করা

ধরেন, আমাদের ওয়েবসাইটে হোম পেজের উপরের দিকে একটা উইজেটে ৩/৪টা **ফিচারর্ড পোস্ট** দেখাব। তাহলে, আমাদের যে সমস্ত পোস্টে দরকার, সেগুলো কাস্টম ফিল্ড দিয়ে Featured করে নিতে হবে। এবার, WP_Query দিয়ে কাস্টম কুয়েরী রান করাতে হবে। এর স্যাম্পল কোড নিচের মত হতে পারে:

```php
$_p = new WP_Query(
	array(
		'posts_per_page' => '',
		'paged' => '',
		'meta_key' => 'featured',
		'meta_value' => '1',
	)
);
```

**নোট:** এই ভিডিওতে মেটা ভ্যালু দিয়ে সার্চ বোঝানোর জন্য সাময়িকভাবে এই উদাহরণ দেখানো হয়েছে। মেটা বক্স নিয়ে পরবর্তী ভিডিওতে বিস্তারিত দেখানো হবে।

### ৯.১১ - WP_Query তে মাল্টিপল মেটা কি এবং মেটা ভ্যালু দিয়ে সার্চ করা

আগের ক্লাসে দেখানো হয়েছে কিভাবে একটি মেটা ডাটা দিয়ে কাস্টম কুয়েরী করা হয়েছে। কিন্তু, যদি একাধিক মেটা ডাটা দিয়ে কাস্টম কুয়েরী করার প্রয়োজন হয়, তবে তার tax_query এর মত আরেকটা আর্গুমেন্ট রয়েছে, meta_query, তা ব্যবহার করতে হবে। সিনট্যাক্সও একই রকম। এই কাস্টম কুয়েরীতে **মাল্টিপল** রিলেশনাল আর্গুমেন্ট ব্যবহার করা যায়, যেমন, প্রথম আর্গুমেন্টের রিলেশন "AND" এবং তারপরে দুইটা আর্গুমেন্ট দেয়া যাবে, যার রিলেশন হতে পারে "OR"।

আগের ক্লাসে ৩টা পোস্টে featured নামে কাস্টম ফিল্ড ব্যবহার করা হয়েছিল। এই ক্লাসে ঐ ৩টা পোস্টের সাথে আরও একটি কাস্টম ফিল্ড যোগ করা হয়, homepage।

```php
$_p = new WP_Query(
	array(
		'posts_per_page' => '',
		'paged' => '',
		'tax_query' => array(
		    'meta_query' => array(
			'relation' => 'AND',
			array(
				'key' => 'featured',
				'value' => '1',
				'compare' => '='
			),
			array(
				'key' => 'homepage',
				'value' => '1',
				'compare' => '='
			)
		)
		)
	)
);
```
