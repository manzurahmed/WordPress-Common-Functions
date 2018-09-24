## ২৩ - শর্টকোড

## ২৩.১ - শর্টকোডের সাথে পরিচিতি

শর্টকোড অত্যন্ত গুরুত্বপূর্ণ বিষয়। এর উপরে ভিত্তি করে বেশির ভাগ থিম ও পেজ বিল্ডারগুলো দাঁড়িয়ে আছে। বাহিরের কনটেন্ট বা কমপ্লেক্স কোন কনটেন্ট এন্ট্রি দেয়ার সিম্পল একটি উপায় হল এই শর্টকোড। শর্টকোড সাধারণত: এন্ড ইউজারদের কাজের সুবিধার জন্য তৈরী করা হয়।

## ২৩.২ - আমাদের প্রথম শর্টকোড

এই পর্বে পোস্টের মধ্যে শর্টকোড দিয়ে বাটন বানানোর উপায় দেখানো হয়েছে। এই শর্টকোডে কিছু প্যারামিটার দিয়ে বাটনের স্টাইলিং পরিবর্তন করা যাবে।

শর্টকোডের কোড **অবশ্যই** থিমের মধ্যে রাখব না। বরং, **plugins** ফোল্ডারের মধ্যে রাখতে হবে।

শর্টকোড ডিক্লেয়ার করতে হয় **add_shortcode** ফাংশন দিয়ে। পোস্টে যে নাম দিয়ে শর্টকোড ট্রিগার করতে ই, সেই নামটিই ফাংশনে ব্যবহার করতে হবে।

মনে রাখতে হবে, যেখানেই শর্টকোড লিখি না কেন, এর সকল প্যারামিটারগুলো ওয়ার্ডপ্রেস ইন্টারনালি প্রসেস করে এ্যারে আকারে এই ফাংশনের কাছে প্রেরণ করে।

এই পর্বে, দুই ভাবে শর্টকোড লিখবার পদ্ধতি দেখানো হয়েছে। একটি কনটেন্ট ছাড়া, এবং, অপরটি কনটেন্ট সহ।

**কনটেন্ট ছাড়া**

```html
[button type="primary" title="Google" url="https://www.webtechriser.com/"]
```

**কনটেন্টসহ**

```html
[button2 target="_blank" type="primary" url="https://www.Yahoo.com/"]Yahoo[/button2]
```

শর্টকোডে কনটেন্ট না থাকলে কলব্যাক ফাংশনে ১টি প্যারামিটার ($attributes) পাঠাতে হয়।

```php
function philosophy_button( $attributes ) {
	return sprintf('<a target="_blank" class="btn btn--%s full-width" href="%s">%s</a>',
		$attributes['type'],
		$attributes['url'],
		$attributes['title']
	);
}
add_shortcode( 'button', 'philosophy_button' );
```

যদি কনটেন্ট থাকে, তবে ২টা প্যারামিটার ($attributes, $content) পাঠাতে হবে।

```php
function philosophy_button2( $attributes, $content ) {
	return sprintf('<a target="_blank" class="btn btn--%s full-width" href="%s">%s</a>',
		$attributes['type'],
		$attributes['url'],
		$content
	);
}
add_shortcode( 'button2', 'philosophy_button2' );
```

## ২৩.৩ - শর্টকোডের ডিফল্ট প্যারামিটার এবং কনটেন্ট

কমার্শিয়াল প্রতিটি শর্টকোডে কিছু ডিফল্ট প্যারামিটার সেট করে দেয়া থাকে। যেমন: যদি Google map এর জন্য আমি একটি শর্টকোড তৈরী করি, যাতে প্যারামিটার রয়েছে ৪টি, যেমন: latitude, longitude, maptype, এবং, marker। এই ৪টি প্যারামিটারই যদি ম্যানিডিটরী হয়, তবে, শর্টকোড লিখার সময় যদি কোন প্যারামিটার লিখতে ভুলে যায়, তবে পিএইচপি এরর দেখাবে।

এ জন্য শর্টকোড ডিফাইন করার সময় কোডে এই ৪টি প্যারামিটারের ডিফল্ট ভ্যালু সেট করে রাখা হয়, যাতে কোন প্যারামিটার না দিলেও ফলব্যাক হিসাবে ডিফল্ট মানটি ব্যবহৃত হয় এবং পিএইচপি এরর’র হাত থেকে বাঁচা যায়।

```
function philosophy_button( $attributes ) {

	// ২৩.৩ - শর্টকোডের ডিফল্ট প্যারামিটার এবং কনটেন্ট
	// প্রথমে ডিফল্ট ভ্যালু সেট করে দিলাম
	$default = array(
		'type' => 'primary',
		'title' => __( 'Button', 'philosophy' ),
		'url' => ''
	);
	// এবার, ইউজার যে সমস্ত প্যারামিটার পাস করেছে, সেগুলোকে ডিফল্ট ভ্যালুর উপরে ওভাররাইট করে দিব
	// এর জন্য ওয়ার্ডপ্রেসের shortcode_atts ফাংশনটি ব্যবহার করব।
	$button_attributes = shortcode_atts( $default, $attributes );

	return sprintf('<a target="_blank" class="btn btn--%s full-width" href="%s">%s</a>',
		$button_attributes['type'],
		$button_attributes['url'],
		$button_attributes['title']
	);
}
add_shortcode( 'button', 'philosophy_button' );
```

## ২৩.৪ - শর্টকোড নেস্টিং

একটা শর্টকোডের মধ্যে আরেকটি শর্টকোড ব্যবহার করাকে ‘নেস্টিং’ বলে। এটা বোঝার জন্য এই পর্বে খুব সাধারণ একটি নতুন শর্টকোড বানানো হয়, যার কাজ হল, $content কে ‘বড় হাতে’ (UPPERCASE) দেখানো।

```php
function philosophy_uppercase( $attributes, $content = '' ) {
	return strtoupper(
		do_shortcode( $content )
	);
}
add_shortcode( 'uc', 'philosophy_uppercase' );
```

এখন এই `uc' শর্টকোর্ডকে যদি আগের পর্বে বানানো **'button2'** শর্টকোডের মধ্যে ব্যবহার করি, তবে দেখা যাবে যে, ওয়েবসাইটে আউটপুটে শর্টকোডটি কাজ করে নাই। শর্টকোডের মধ্যে নেস্টিং আরেকটা শর্টকোড ব্যবহার করলে $content কে do_shortcode ফাংশনের মধ্যে দিয়ে পাস করাতে হবে। অর্থাৎ, $content এর মধ্যে যদি শর্টকোড ব্যবহার করা হয়, তবে সেগুলোকে আগে আউটপুটে নিয়ে এসে তারপর হায়ারারকি’র আগের শর্টকোডকে কার্যকরী করবে।

```php
function philosophy_button2( $attributes, $content = '' ) {

	// ২৩.৩ - শর্টকোডের ডিফল্ট প্যারামিটার এবং কনটেন্ট
	// প্রথমে ডিফল্ট ভ্যালু সেট করে দিলাম
	$default = array(
		'type' => 'primary',
		'url' => ''
	);
	// এবার, ইউজার যে সমস্ত প্যারামিটার পাস করেছে, সেগুলোকে ডিফল্ট ভ্যালুর উপরে ওভাররাইট করে দিব
	// এর জন্য ওয়ার্ডপ্রেসের shortcode_atts ফাংশনটি ব্যবহার করব।
	$button_attributes = shortcode_atts( $default, $attributes );

	return sprintf('<a target="_blank" class="btn btn--%s full-width" href="%s">%s</a>',
		$button_attributes['type'],
		$button_attributes['url'],
		do_shortcode( $content )
	);
}
add_shortcode( 'button2', 'philosophy_button2' );
```

### বিশেষ দ্রস্টব্য:

কোন শর্টকোডে প্যারামিটার হিসাবে $content ব্যবহার করা হলে **অবশ্যই** do_shortcode($content) ফাংশন ব্যবহার করতে হবে।

## ২৩.৫ - গুগল ম্যাপস শর্টকোড

এই ভিডিওতে গুগল ম্যাপের জন্য একটা শর্টকোড তৈরী করার পদ্ধতি দেখানো হয়। এখানে গুগল ম্যাপকে এমবেড করা হয়েছে। যে কারণে গুগল ম্যাপস এর কোন এপিআই লাগবে না।

পোস্টের মধ্যে [gmap] শর্টকোড লিখলেই ওয়েবসাইটে একটি গুগল ম্যাপ দেখাবে। শর্টকোডটিতে place, zoom, width, height - এই ৪টি ডিফল্ট প্যারামিটার ব্যবহার করা হয়েছে। gmap শর্টকোডের সাথে ৪টি প্যারামিটারের মধ্যে যে কোন সংখ্যায় প্যারামিটার লিখলেই হবে। যেমন: [gmap place="Dhaka" width="100%" height="200" zoom=13]

```php
function philosophy_google_map( $attributes ) {
	$default = array(
		'place' => 'Dhaka Museum',
		'zoom' => 13,
		'width' => 800,
		'height' => 500
	);

	$params = shortcode_atts( $default, $attributes );

	// heredoc
	$map =
<<<EOD
<div>
	<div>
		<iframe width="{$params['width']}" height="{$params['height']}"
			src="https://maps.google.com/maps?q={$params['place']}&t=&z={$params['zoom']}&ie=UTF8&iwloc=&output=embed"
			frameborder="0" scrolling="no" margineheight="0" marginwidth="0">
		</iframe>
	</div>
</div>
EOD;

	return $map;
}
add_shortcode( 'gmap', 'philosophy_google_map' );
```
