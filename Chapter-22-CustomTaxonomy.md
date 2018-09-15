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
