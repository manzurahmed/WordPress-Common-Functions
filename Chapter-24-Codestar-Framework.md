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

## ২৪.৩ - কোডস্টার ফ্রেমওয়ার্কের ফিল্ড ডিপেন্ডেন্সি (খুবই ইন্টারেস্টিং)

কোডস্টার ফ্রেমওয়ার্কের “ডিপেনডেনসি” একটি অত্যান্ত শক্তিশালী ফিচার। কোডস্টারের মেটাবক্স ব্যবহার করে পেজ, পোস্ট, অপশন প্যানেল, ট্যাক্সোনমি, কাস্টোমাইজার - বিভিন্ন স্থানে ইউজারদেরকে অপশন কন্ট্রোল প্যানেল তৈরী করে দেয়া যায়।

অপশন প্যানেলে টেক্সট বক্স, চেকবক্স, সুইচার, রেডিও বাটন, ড্রপডাউন, প্রভৃতি থাকতে পারে। এদের এক বা একাধিক কন্ট্রোলের ভ্যালুর উপরে ভিত্তি করে আরেকটি কন্ট্রোলকে show বা hide করতে “ডিপেনডেনসি” ব্যবহার করা হয়।

উদাহরণ হিসাবে ধরা যায় যে, আমার "Is Favorite" সুইচার বাটন এবং "Favorite Text" নামে টেক্সবক্স আছে। "Is Favorite" সুইচার বাটনকে ক্লিক করলেই কেবল "Favorite Text" টেক্সটবক্সটি দেখাবে। অন্যথায় দেখাবে না। এ ক্ষেত্রে নিচের কোড লিখতে হবে:

```php
array(
	'id' => 'is-favorite',
	'type' => 'switcher', // Codestar type
	'title' => __( 'Is Favorite', 'philosophy'),
	'default' => 1
),
// FOR SINGLE DEPENDENCY, USE THIS
array(
	'id' => 'page-favorite-text',
	'type' => 'text', // Codestar type
	'title' => __( 'Favorite Text', 'philosophy'),
	'dependency' => array(
		'is-favorite',
		'==',
		'1'
	)
),
```

**২টা সুইচারের ভ্যালুর উপরে ভিত্তি করে টেক্সটবক্স দেখানো**

যদি ২টা কন্ট্রোলের ভ্যালুর উপরে "Favorite Text" কে দেখাতে হয়, তখন ডিপেনডেনসি চেক করার পদ্ধতিটি খুব সহজ। ধরি, "Is Favorite" সুইচার বাটন এর নীচে "Is Favorite Extra" নামে আরেকটা সুইচার বাটন তৈরী করা হল। এই দু’টো বাটন **একসাথে সিলেক্ট** করলেই কেবল "Favorite Text" টেক্সটবক্স দেখা যাবে। এ ক্ষেত্রে কোড হবে নিম্নরূপ:

```php
// FOR MULTIPLE DEPENDENCY, USE THIS
array(
	'id' => 'page-favorite-text',
	'type' => 'text', // Codestar type
	'title' => __( 'Favorite Text', 'philosophy'),
	'dependency' => array(
		'is-favorite|is-favorite-extra',
		'==|==',
		'1|1'
	)
),
```

**Checkbox এর ২টা অপশনের উপর ভিত্তি করে টেক্সটবক্স দেখানো**

ধরি, ভাষা সিলেক্ট করার একটি চেকবক্স আছে, যাতে ৩টা ভাষার অপশন আছে, bangla, english এবং french। “একসাথে বাংলা ও ইংরেজী” ভাষা দু’টো সিলেক্ট করলে “extra-language-data” নামের একটি টেক্সটবক্স দেখাবে। এ ক্ষেত্রে কোড হবে:

```php
// Checkbox
array(
	'id' => 'support-language',
	'type' => 'checkbox',
	'title' => __( 'Languages', 'philosophy' ),
	'options' => array(
		'bangla' => 'Bangla',
		'english' => 'English',
		'french' => 'French'
	)
),
array(
	'id' => 'extra-language-data',
	'type' => 'text', // Codestar type
	'title' => __( 'Extra Language Data', 'philosophy'),
	'dependency' => array(
		'support-language_bangla|support-language_english',
		'==|==',
		'1|1'
	)
),
```

কিন্তু, যদি bangla ও english - **যে কোন ১টা** ভাষা সিলেক্ট করলে টেক্সটবক্সটা দেখাবে, সে ক্ষেত্রে কোডস্টারের কোডের স্টাইল একটু ভিন্ন:

```php
// আর, যে কোন ১টা ভাষা সিলেক্ট করলেই টেক্সটবক্স দেখাবে
array(
	'id' => 'support-language',
	'type' => 'checkbox',
	'title' => __( 'Languages', 'philosophy' ),
	'options' => array(
		'bangla' => 'Bangla',
		'english' => 'English',
		'french' => 'French'
	),
	// 'any' ব্যবহার করলে এই 'attributes' প্যারামিটার ব্যবহার করতে হবে
	'attributes' => array(
		'data-depend-id' => 'support-language'
	)
),
array(
	'id' => 'extra-language-data',
	'type' => 'text', // Codestar type
	'title' => __( 'Extra Language Data', 'philosophy'),
	'dependency' => array( 'support-language','any','bangla, english' )
),
```
