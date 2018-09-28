# ২৪ - কোডস্টার ফ্রেমওয়ার্ক

## ২৪.১ - কোডস্টার ফ্রেমওয়ার্কের সাথে পরিচয়

কোডস্টার ফ্রেমওয়ার্ককে (Codestar Framework) থিমের সাথে দুইভাবে ব্যবহার করা যায়।

1. TGMPA দিয়ে required plugin হিসাবে, অথবা,
2. ডাউনলোড করে থিমের মধ্যে যুক্ত করে নেয়া।

এই ফ্রেমওয়ার্ক প্লাগইন আকারে ইন্সটল না করে থিমের সাথে লাইব্রেরী আকারে ব্যবহার করার অনেক বাড়তে সুবিধা আছে। কোডস্টার ফ্রেমওয়ার্ক’র গিটহাব লোকেশন (https://github.com/Codestar/codestar-framework/) থেকে ডাউনলোড করে এর সব ফাইল থিমের "lib" ফোল্ডারে "csf" নামে নতুন ফোল্ডারের মধ্যে কপি করে দিতে হবে।

শেষে, functions.php ফাইলে কোডস্টার ফ্রেমওয়ার্ককে require_once ফাংশন দিয়ে যুক্ত করে নিতে হবে।

```php
require_once get_theme_file_path( '/lib/codestar/cs-framework.php' );
```

## ২৪.২ - কোডস্টার ফ্রেমওয়ার্ক দিয়ে মেটাবক্স বানানো

এই পর্বে কোডস্টার ফ্রেমওয়ার্ক দিয়ে মেটাবক্স বানানোর পদ্ধতি দেখানো হয়েছে। এর আগের পর্বে lib ফোল্ডারের codestar ফোল্ডারে কোডস্টার ফ্রেমওয়ার্ক প্লাগইনের সব ফাইল কপি-পেস্ট করে রেখেছি। এবার, inc ফোল্ডারে cs.php নামে একটি ফাইল তৈরী করে তার ভিতরে নিচের কোডগুলো লিখতে হবে।

প্রথমেই কোডস্টারের কিছু ডিফল্ট ভ্যালু সেট করতে হবে। যেমন: এই পর্বে শুধু মেটাবক্স তৈরী করা হয়েছে, তাই, CS_ACTIVE_METABOX কনস্ট্যান্টের ভ্যালু true সেট করা হয়েছে এবং অন্যান্য ভ্যালুগুলোকে false করে রাখা হয়েছে।

কোডস্টারের ডকুমেন্টেশনে বিস্তারিত জানা যাবে: https://github.com/Codestar/codestar-framework/

```php
<?php
// ২৪.২ - কোডস্টার ফ্রেমওয়ার্ক দিয়ে মেটাবক্স বানানো
define( 'CS_ACTIVE_FRAMEWORK',   false  ); // default true
define( 'CS_ACTIVE_METABOX',     true ); // default true
define( 'CS_ACTIVE_TAXONOMY',    false ); // default true
define( 'CS_ACTIVE_SHORTCODE',   false ); // default true
define( 'CS_ACTIVE_CUSTOMIZE',   false ); // default true


function philosophy_page_metabox() {
	$options[] = array(
		'id' => 'page-metabox',
		'title' => __( 'Page Meta Info', 'philosophy' ),
		'post_type' => 'page',
		'context' => 'normal',
		'priority' => 'default',
		'sections' => array(
			array(
				'name' => 'page-section1',
				'title' => __( 'Page Secttings', 'philosophy' ),
				'icon' => 'fa fa-image',
				'fields' => array(
					array(
						'id' => 'page-heading',
						'type' => 'text', // Codestar type
						'default' => __( 'Page Heading', 'philosophy' )
					),
					array(
						'id' => 'page-teaser',
						'type' => 'textarea', // Codestar type
						'default' => __( 'Teaser Text', 'philosophy' )
					),
					array(
						'id' => 'is-favorite',
						'type' => 'switcher', // Codestar type
						'default' => 1
					),
				)
			)
		)
	);

	return $options;
}
add_filter( 'cs_metabox_options', 'philosophy_page_metabox' );
```

যে কোন ধরনের মেটাবক্সের লেবেলগুলো কালো রঙে দেখায়। কোডস্টার ফ্রেমওয়ার্কে Light theme দেয়াই আছে। ইচ্ছা করলে এই থিম ব্যবহার করা যায়। এটা ব্যক্তিগত অভিরুচির ব্যাপার। লাইট থিম ব্যবহারের জন্য নিচের কোড লিখব:

```php
define( 'CS_ACTIVE_LIGHT_THEME',  true  ); // default false
```

**কোডস্টার মেটাবক্সের ডাটা ওয়েবসাইটে দেখান**

কোডস্টার মেটাবক্সের ডাটা মেটাবক্সের আইডি (আমাদের উদাহরণের ক্ষেত্রে, page-metabox) দিয়ে ঐ পেজের “পেজমেটা” হিসাবে সেভ হয়। আর, আমি এই ক্লাসের জন্য Abous US পেজে মেটাডাটা যুক্ত করেছিলাম। তাই, About US পেজের পেজ টেমপ্লেট about.php ফাইল ব্যবহার করে মেটাডাটা কোড দিয়ে বের করে ওয়েবসাইটে দেখাব।

```php
<h3><?php echo __( 'Example: Codestar Framework Metabox for Page', 'philosophy' ); ?></h3>
<?php
// ২৪.২ - কোডস্টার ফ্রেমওয়ার্ক দিয়ে মেটাবক্স বানানো
$philosophy_cs_page_meta = get_post_meta( get_the_ID(), 'page-metabox', true );
// Page headline
echo esc_html( $philosophy_cs_page_meta['page-heading'] ) . '<br />';
echo esc_html( $philosophy_cs_page_meta['page-teaser'] ) . '<br />';

if( $philosophy_cs_page_meta['is-favorite'] ) {
    echo esc_html( __( 'This is favorite' , 'philosophy' ) );
} else {
    echo esc_html( __( 'This is NOT favorite' , 'philosophy' ) );
}
?>
```
