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

## ২৪.৪ - বিভিন্ন কন্ডিশনের উপরে ভিত্তি করে কোডস্টার মেটাবক্সগুলো দেখানো

কোডস্টার দিয়ে বানানো কোন একটি মেটাবক্স যদি পোস্ট বা পেজ এর বানানো হয়, তবে সেই মেটাবক্স সব পোস্ট বা পেজ এ দেখায়। এটাই **ডিফল্ট বিহ্যাভিওর**।

কিন্তু, যদি কোন একটি মেটাবক্সকে যদি নির্দিষ্ট কোন পোস্ট, বা, পেজে, বা, পেজের নির্দিষ্ট কোন টেমপ্লেটে দেখাতে চাই, তাহলে থিমের inc ফোল্ডারের cs.php ফাইলের ফাংশন philosophy_page_metebox($options) এর মধ্যে চেক করে দেখতে হবে, আমি এ্যাডমিনে কোন সেকশনে আছি, পেজ এডিটিং এ, নাকি, পোস্ট এডিটিং এ। একই সাথে চেক করে দেখতে হবে যে, আমি যে পেজে আছি সেই পেজের সাথে বর্তমানে কোন টেমপ্লেটা এ্যাসাইন করা আছে।

```php
// This condition checks which Admin Section I am now
$page_id = 0; // Must initialize this value to "0". Otherwise, an PHP error will occur when I navigate to other section of WordPress Admin area.
if( isset( $_REQUEST['post'] ) || isset( $_REQUEST['post_ID'] ) ) {
	$page_id = empty( $_REQUEST['post_ID'] ) ? $_REQUEST['post'] : $_REQUEST['post_ID'];
}
// Now, get the Currently Assigned "Page Template" of this page
$current_page_template = get_post_meta( $page_id, '_wp_page_template', true );
// echo $current_page_template;
// wp_die();

if( 'about.php' != $current_page_template ) {
	return $options;
}
```

কিন্তু, যদি একাধিক পেজ টেমপ্লেটের উপস্থিতি চেক করে দেখতে চাই, তবে, in_array ফাংশন ব্যবহার করে একাধিক টেমপ্লেটের নাম আর্গুমেন্ট আকারে পাঠিয়ে চেক করতে হবে।

```php
if( !in_array( $current_page_template, array( 'about.php', 'contact.php' ) ) ) {
	return $options;
}
```

## ২৪.৫ - কোডস্টার ফ্রেমওয়ার্কের আপলোড ফিল্ড এবং ইমেজ ফিল্ড

এই পর্বে কোডস্টার ফ্রেমওয়ার্কে upload এবং image ফিল্ড এর ব্যবহার দেখানো হয়েছে।

- upload - পিডিএফ, বা, ভিডিও, অডিও আপলোড করার জন্য এই ফিল্ড টাইপ ব্যবহার করা হয়।
- image - ছবি বা ইমেজ আপলোড করার জন্য এই ফিল্ড টাইপ ব্যবহার করা হয়।

upload ফিল্ড টাইপ থেকে যেটা আপলোড করা হয়েছে, তার সম্পূর্ণ url রিটার্ন করে ( http://localhost/alpha/wp-content/uploads/2018/10/Orygami-f14a_inst.pdf)। 
image ফিল্ড টাইপ থেকে ইমেজের ID রিটার্ন করে। এই আইডি ব্যবহার করে wp_get_attachment_image ফাংশন দিয়ে ইমেজকে গ্রাফিকালি দেখানো যায়। আর, যদি ইমেজের url প্রয়োজন হয়, তবে wp_get_attachment_image_src ফাংশন ব্যবহার করতে হবে।

প্রথমে cs.php ফাইলে আরেকটি add_filter ব্যবহার করে নতুন মেটাবক্স বানাব।

```php
function philosophy_upload_metabox( $options ) {
	
	$options[] = array(
		'id' => 'page-upload-metabox',
		'title' => __( 'Upload files', 'philosophy' ),
		'post_type' => 'page',
		'context' => 'normal',
		'priority' => 'default',
		'sections' => array(
			array(
				'name' => 'page-section1',
				'title' => __( 'Upload Files', 'philosophy' ),
				'icon' => 'fa fa-image',
				'fields' => array(

					// Upload
					// http://codestarframework.com/documentation/#upload
					array(
						'id'            => 'page-upload',
						'type'          => 'upload',
						'title'         => __( 'Upload PDF', 'philosophy' ),
						'settings'      => array(
							'upload_type'  => 'application/pdf',
							'button_title' => __( 'Upload', 'philosophy' ),
							'frame_title'  => __( 'Select an PDF', 'philosophy' ),
							'insert_title' => __( 'Use this PDF', 'philosophy' ),
						)
					),
					array(
						'id'            => 'page-image',
						'type'          => 'image',
						'title'         => __( 'Upload Image', 'philosophy' ),
						'add_title'		=> __( 'Add an Image', 'philosophy' )
					)
				)
			)
		)
	);

	return $options;
}
add_filter( 'cs_metabox_options', 'philosophy_upload_metabox' );
```

কোডস্টারের upload এবং image ফিল্ড টাইপের ব্যবহার দেখানার জন্য আলাদা নুতন একটি পেজ তৈরী করা হয় এবং এর আউটপুটকে আলাদাভাবে দেখানোর জন্য স্পেসিফিক একটা php ফাইল তৈরী করা হয়। নতুন পেজের permalink এর address ছিল "codestar-upload-example"। তাই, page-codestar-upload-example.php নামে একটি ফাইল তৈরী করে তার মধ্যে page.php ফাইল এর কনটেন্ট পেস্ট করে নিব।

upload ফিল্ড টাইপের ডাটা মেটাবক্স থেকে পুল করে আনতে নিচের কোড ব্যবহার করব:

```php
$philosophy_page_meta = get_post_meta( get_the_ID(), 'page-upload-metabox', true );
print_r( $philosophy_page_meta['page-upload'] );
```

আর, image ফিল্ডের ডাটা পুল করার জন্য নিচের কোড ব্যবহার করব।:

```php
print_r( $philosophy_page_meta['page-image'] );
                echo '<br />';
                echo wp_get_attachment_image( $philosophy_page_meta['page-image'], 'medium' );
                echo '<br />';
                echo wp_get_attachment_image_src( $philosophy_page_meta['page-image'], 'medium' )[0];
```

## ২৪.৬ - কোডস্টার ফ্রেমওয়ার্কের গ্যালারী ফিল্ড

এই পর্বে কোডস্টার ফ্রেমওয়ার্কের গ্যালারী ফিল্ড টাইপের ব্যবহার দেখানো হয়েছে। আগের টিউটোরিয়ালে ব্যবহার করা ইমেজ ফিল্ডের নীচে গ্যালারী ফিল্ড ব্যবহার করা হয়। এই ফিল্ডের মাধ্যমে একাধিক ইমেজ ব্যবহার করা যায়। একবার এ্যাড করার পর "Add Images" বাটন দিয়ে গ্যালারীতে নতুন ইমেজ যুক্ত করা যাবে। "Edit images" বাটন ব্যবহার করে গ্যালারীর ইমেজগুলোকে ড্র্যাগ করে রিঅ্যারাঞ্জ করা যাবে।

![Codestar Framework Gallery Field Style](https://github.com/manzurahmed/WordPress-Common-Functions/blob/master/images/chapter-24/codestar-gallery-field-type.jpg)

গ্যালারী ফিল্ডের জন্য cs.php ফাইলে philosophy_upload_metabox ফাংশনের মধ্যে নীচের কোডগুলো লিখতে হবে:

```php
array(
						'id'          => 'page-gallery',
						'type'        => 'gallery',
						'title'       => __( 'Upload Image', 'philosophy' ),
						'add_title'   => __( 'Add Images', 'philosophy' ),
						'edit_title'  => __( 'Edit Images', 'philosophy' ),
						'clear_title' => __( 'Clear Gallery', 'philosophy' ),
					),
```

ওয়ার্ডপ্রেস এ্যাডমিন প্যানেল থেকে গ্যালারী ফিল্ডে কিছু ইমেজ যুক্ত করে ওয়েবসাইটে দেখানো জন্য page-codestar-upload-example.php ফাইলে নীচের কোডগুলো যুক্ত করি। ইমেজগুলোকে TinySlider লাইব্রেরী ব্যবহার করে দেখানো হয়েছে (যদিও সিএসএস ঠিক করা হয়নি)।

```php
if( !empty( $philosophy_page_meta['page-gallery'] ) ) {
    $philosophy_gallery_ids = explode( ",", $philosophy_page_meta['page-gallery'] );

    // Tiny Sider
    // https://github.com/ganlanyuan/tiny-slider
    ?>
    <div class="my-slider">
    <?php
    foreach( $philosophy_gallery_ids as $philosophy_gallery_id ) { ?>
	<div>
	    <a href="#">
		<?php echo wp_get_attachment_image( $philosophy_gallery_id, 'large' ); ?>
	    </a>
	</div>
    <?php
    }
    ?>
    </div>
<?php
}
?>
```

## ২৪.৭ - কোডস্টার ফ্রেমওয়ার্কের ফিল্ডসেট ফিল্ড

ফিল্ডসেট হলো, একই টাইপের ফিল্ডগুলোকে গ্রুপ করার একটি HTML ট্যাগ। এই ভিডিও কোডস্টারের ডকুমেন্টেশন থেকে ফিল্ডসেটের ডিফল্ট কোড ব্যবহার করা হয়েছে, তবে, আপলোড ফিল্ডটা কেটে দেয়া হয়। print_r ব্যবহার করে দেখতে পাই যে, ফিল্ডসেট array আকারে এর ভ্যালুগুলো রিটার্ন করে।

![Fieldset fieldtype](https://github.com/manzurahmed/WordPress-Common-Functions/blob/master/images/chapter-24/codestar-fieldset-fieldtype.jpg)

```php
array(
	'id'        => 'fieldset_1',
	'type'      => 'fieldset',
	'title'     => 'Fieldset Field',
	'fields'    => array(
	  array(
		'id'    => 'fieldset_1_text',
		'type'  => 'text',
		'title' => 'Text Field',
	  ),
	  array(
		'id'    => 'fieldset_1_textarea',
		'type'  => 'textarea',
		'title' => 'Textarea Field',
	  ),
	),
),
```

এই এ্যারে থেকে যে কোন একটি ভ্যালু পেতে হলে, এ্যারের এলিমেন্ট ধরে কল করতে হবে, যেমন:

```php
echo $philosophy_page_meta['fieldset_1']['fieldset_1_text'];
```

# ২৪.৮ - কোডস্টার ফ্রেমওয়ার্কের রিপিটেবল গ্রুপ ফিল্ড এবং কিছু ট্রিকস

কোডস্টারের group ফিল্ড টাইপ দিয়ে যে কোন ফিল্ডকে রিপিট করা যায়। যেমন: টেসটিমোনিয়াল এ একই ধরণের তথ্য একাধিক নেয়ার দরকার হয়। এর ইনপুট নেয়ার জন্য গ্রুপ ফিল্ড ব্যবহার করা যায়। এই ভিডিওতে কোডস্টারের উদাহরণে থাকা কোড নিয়ে গ্রুপ ফিল্ডকে দেখানো হয়েছে।

```
array(
  'id'              => 'unique_option_901',
  'type'            => 'group',
  'title'           => 'Group Field',
  'button_title'    => 'Add New',
  'accordion_title' => 'Add New Field',
  'fields'          => array(
    array(
      'id'    => 'unique_option_901_text_1',
      'type'  => 'text',
      'title' => 'Text Field 100',
    ),
    array(
      'id'    => 'unique_option_901_upload_1',
      'type'  => 'upload',
      'title' => 'Upload Field',
    ),
    array(
      'id'    => 'unique_option_901_switcher_1',
      'type'  => 'switcher',
      'title' => 'Switcher Field',
    ),
  ),
),
```

পরে, কোডস্টারের ডিফল্ট উদাহরণের কোড মুছে নতুন করে একটি গ্রুপ ফিল্ডের কোড লেখা হয়, যেখানে একটি select ফিল্ডকে রিপিট করা হয় এবং সেখানে প্রতিটি পেজের লিস্ট দেখাবে। **এখানে একটু বুঝতে হবে, কি করা হয়েছে**

```php
'fields'          => array(
	array(
		'id' => 'featured_posts',
		'type' => 'select',
		'title' => __( 'Select a page', 'philosophy' ),
		'options' => 'pages'
	)
),
```

এই মেটাবক্সটি পরীক্ষা করার জন্য এ্যাডমিনে গিয়ে CodeStar Upload Example পেজটি রিফ্রেস করে group ফিল্ডে ৩/৪ টা এন্ট্রি দিয়ে পেজ আপডেট (সেভ) করে দিব। এতে দেখা যাবে, গ্রুপ ফিল্ডে মেটাবক্সের টাইটেল হিসাবে মেটাবক্সের আইডি দেখাচ্ছে।

![Codestar metabox group field: Metabox ID as title](https://github.com/manzurahmed/WordPress-Common-Functions/blob/master/images/chapter-24/Group%20field%20with%20id%20as%20title.jpg)

কিন্তু, টাইটেল হিসাবে পেজের বা পোস্টের বা কাস্টম পোস্টের টাইটেল দেখালে ব্যবহারকারীর জন্য সুবিধা হবে। এ জন্য কোডস্টারের group.php ফাইল **মডিিই** করা হয়েছে। ফাইলের ৫৮ নম্বর লাইনে ৩টা লাইন যুক্ত করা হয়েছে। ফাইল লোকেশন: "lib/cs-framework/fields/group/group.php"।

```php
if( $field_id == 'featured_posts' ) {
    $title = get_the_title( $this->value[$key][$field_id] );
  }
```

পরবর্তীতে, পেজের লিস্টের পরবর্তীতে book এর লিস্ট দেখানো হয়। কোডস্টারের ডকুমেন্টেশনে select ফিল্ডের জন্য কিভাবে কুয়েরী পাস করতে হয়, সেটা বলে দেয়া আছে।

```php
array(
	'id' => 'featured_posts',
	'type' => 'select',
	'title' => __( 'Select a page', 'philosophy' ),
	'options' => 'posts',
	'query_args'     => array(
		'post_type'    => 'book',
		'posts_per_page' => -1,
		'orderby'      => 'post_date',
		'order'        => 'DESC',
	),
)
```

![Codestar metabox group field: Post title as title](https://github.com/manzurahmed/WordPress-Common-Functions/blob/master/images/chapter-24/group%20field%20with%20post%20title%20as%20title.jpg)

## ২৪.৯ - আরো অ্যাডভান্সড কন্ডিশনের উপরে ভিত্তি করে কোডস্টার মেটাবক্স ডিসপ্লে করা

বিভিন্ন কন্ডিশনের উপরে ভিত্তি করে মেটাবক্স কিভাবে দেখানো হয়, এই পর্বে তা দেখানো হয়েছে। এই পর্বে কোডস্টার দিয়ে একটা আলাদা মেটাবক্স বানানো হয়, যেখানে ৩টি ইনপুট ফিল্ড থাকে। এক নম্বরে রয়েছে, আগের পর্বে বানানো একটি সিলেক্ট বক্সে কাস্টম পোস্ট টাইপগুলো। Book এবং Chapter। বুক সিলেক্ট করলে বুকের জন্য অপশন ফিল্ডটি দেখাবে। আর, চ্যাপ্টার সিলেক্ট করলে, চ্যাপ্টারের জন্য বানানো অপশন ফিল্ডটি দেখাবে।

উল্লেখ্য, বুক বা চ্যাপ্টার সিলেক্ট করার পর আমাদের উদাহরণের জন্য বানানো পেজটা সেভ করার পর তাদের সাথে সংশ্লিষ্ট অপশন ফিল্ডটি দেখাবে।

এখানে, আগে $page_id বের করে নেয়া হচ্ছে। তারপর $page_meta_info বের করে নিয়ে, তার উপরে ভিত্তি করে সংশ্লিষ্ট অপশন ফিল্ডটি দেখানো হচ্ছে। **খুব সিম্পল একটা ব্যাপার। জানতে সহজ, না পারলে প্যারা।**

কোড উদাহরণ:

```php
// ২৪.৯ - আরো অ্যাডভান্সড কন্ডিশনের উপরে ভিত্তি করে কোডস্টার মেটাবক্স ডিসপ্লে করা
function philosophy_custom_post_types( $options ) {

	// This condition checks which Admin Section I am now
	$page_id = 0;
	if( isset( $_REQUEST['post'] ) || isset( $_REQUEST['post_ID'] ) ) {
		$page_id = empty( $_REQUEST['post_ID'] ) ? $_REQUEST['post'] : $_REQUEST['post_ID'];
	}

	$options[] = array(
		'id' => 'page_custom_post_type',
		'title' => __( 'Select Post Type', 'philosophy' ),
		'post_type' => 'page',
		'context' => 'normal',
		'priority' => 'default',
		'sections' => array(
			array(
				'name' => 'page-section1',
				// 'title' => __( 'Post Types', 'philosophy' ),
				'icon' => 'fa fa-image',
				'fields' => array(

					array(
						'id' => 'cpt_type',
						'type' => 'select',
						'title' => __( 'Select a custom post type', 'philosophy' ),
						'options' => array(
							'none' => 'None',
							'book' => 'Book',
							'chapter' => 'Chapter'
						)
					)

				)
			)
		)
	);

	// 
	$page_meta_info = get_post_meta ( $page_id, 'page_custom_post_type', true);

	if( isset( $page_meta_info['cpt_type'] ) && $page_meta_info['cpt_type'] == 'book' ) {
		$options[] = array(
			'id' => 'page-custom_post_type_book',
			'title' => __( 'Options for Book', 'philosophy' ),
			'post_type' => 'page',
			'context' => 'normal',
			'priority' => 'default',
			'sections' => array(
				array(
					'name' => 'page-section1',
					// 'title' => __( 'Post Types', 'philosophy' ),
					'icon' => 'fa fa-image',
					'fields' => array(
	
						array(
							'id' => 'option_book_text',
							'type' => 'text',
							'title' => __( 'Some Book Info', 'philosophy' ),
						)
	
					)
				)
			)
		);
	}

	if( isset( $page_meta_info['cpt_type'] ) && $page_meta_info['cpt_type'] == 'chapter' ) {
		$options[] = array(
			'id' => 'page-custom_post_type_chapter',
			'title' => __( 'Options for Chapter', 'philosophy' ),
			'post_type' => 'page',
			'context' => 'normal',
			'priority' => 'default',
			'sections' => array(
				array(
					'name' => 'page-section1',
					// 'title' => __( 'Post Types', 'philosophy' ),
					'icon' => 'fa fa-image',
					'fields' => array(

						array(
							'id' => 'option_chapter_text',
							'type' => 'text',
							'title' => __( 'Some Chapter Info', 'philosophy' ),
						)

					)
				)
			)
		);
	}

	return $options;
}

add_filter( 'cs_metabox_options', 'philosophy_custom_post_types' );
```

![Display metabox upon condition](https://github.com/manzurahmed/WordPress-Common-Functions/blob/master/images/chapter-24/SelectPostType_All_Options.jpg)

## ২৪.১০ - কোডস্টার দিয়ে শর্টকোড এন্ট্রি দেয়ার ফর্ম বানানো 

এই পর্বে কোডস্টার ফ্রেমওয়ার্ক দিয়ে শর্টকোড এর জন্য ভিজ্যুয়াল এডিটর বানানো যায়।

প্রথমেই, কোডস্টার ফ্রেমওয়ার্ক এর একটি “ডিফাইন সুইচ” **CS_ACTIVE_SHORTCODE** সুইচকে **true** করে দিতে হবে। এটা পাওয়া যাবে inc ফোল্ডারের cs.php ফাইলে। এর নীচে থাকা philosophy_csf_metabox নামের ফাংশন কলব্যাকে নীচের লাইনটি যোগ করতে হবে।

```php
CSFramework_Shortcode_Manager::instance( array() );
```

### এখানে একটা সমস্যা হচ্ছে। এডিটরের টুলবার সর্টকোর্ডের ভিজুয়াল এডিটরের উপরে উঠে যাচ্ছে। এটার একটা ফিক্স দরকার।

```php
function philosophy_cs_google_map( $options ) {

	$options      = array();

	$options[]     = array(
		'name'          => 'group_1',
		'title'         => __( 'Group #1', 'philosophy' ),
		'shortcodes'    => array(
	  
		  array(
			'name'      => 'gmap',
			'title'     => __( 'Google Map', 'philosophy' ),
			'fields'    => array(
			  array(
				'id'    => 'place',
				'type'  => 'text',
				'title' => __( 'Place', 'philosophy' ),
				'help'  => __( 'Enter place name.', 'philosophy' ),
				'default' => 'Mogbazar, Dhaka'
			  ),
			  array(
				'id'    => 'width',
				'type'  => 'text',
				'title' => __( 'Width', 'philosophy' ),
				'default' => '100%'
			  ),
			  array(
				'id'    => 'height',
				'type'  => 'text',
				'title' => __( 'Enter Height', 'philosophy' ),
				'default' => 500
			  ),
			  array(
				'id'    => 'zoom',
				'type'  => 'text',
				'title' => __( 'Enter Zoom value.', 'philosophy' ),
				'default' => 14
			  )
			),
		  ),
		)
	);

	return $options;
}
add_filter( 'cs_shortcode_options', 'philosophy_cs_google_map' );
```

