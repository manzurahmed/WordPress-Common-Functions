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
