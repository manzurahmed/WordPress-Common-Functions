# হুক তৈরী করা এবং হুক নিয়ে আলোচনা

## ১৯.১ - অ্যাকশন হুক নিয়ে বিস্তারিত, সাথে ওয়ার্ডপ্রেস অপশন টেবিলে ডেটা সেভ করা

এ্যাকশন হুকে একটা ইভেন্ট ডিসপ্যাচ করা হয়। অর্াৎ ইভেন্ট ব্রডকাস্ট করা হয় যে, একটা কিছু ঘটেছে, আপনারা এখন কিছু একটা কর।
ফিল্টার হুকে - ইভেন্ট তো দেয়ায় হয়, সাথে কিছু ডাটাও দেয়া হয়।

যেমন: category.php ফাইলে ক্যাটাগরী টাইটেলের নিচে যদি নিচের do_action ব্যবহার করা হয়, তবে এখানে একটা ইভেন্ট ব্রডকাস্ট হবে, এবং কলব্যাক ফাংশনে লেখা কাজগুলো চালাবে।

```php
do_action( "philosophy_after_title_display" );
```

এখন, বায়ার পারপেক্টিভে চিন্তা করি। আমার থিম কিনে বায়ার দেখল, আরে ক্যাটাগরী টাইটেল ও ডেসক্রিপশনের উপরে ও নীচে do_action এ্যাকশন দেয়া আছে। তাহলে, তিনি যদি এখানে কোন কিছু যুক্ত করতে চান, তাহলে functions.php ফাইলে গিয়ে ঐ do_action এ থাকা এ্যাকশনের নাম দিয়ে একটা add_action লিখে ফেললেই হবে। আমার থিমের কোন কোড পরিবর্তন না করলেও চলবে।

```php
function after_title_display() {
	echo "<p>After Title</p>";
}
add_action( "philosophy_after_title_display", "after_title_display" );
```

category.php ফাইলের উপর আরেকটা do_action হুক যুক্ত করা হয়েছে। এর আর্গুমেন্টের দ্বিতীয় প্যারামিটারে ক্যাটাগরীর নাম পাস করা হচ্ছে। এই নামের কোন ক্যাটাগরী পেজে আসলে একটা করে হিট কাউন্টার বাড়ানো হবে। অর্থাৎ ঐ ক্যাটাগরী কি রকম ভিউ হচ্ছে তার স্ট্যাট সংগ্রহ করা হচ্ছে। আমার ওয়ার্ডপ্রেসের অপশন টেবিলের নাম ছিল, alp_options। functions.php ফাইলে philosophy_category_page অ্যাকশন হুকের জন্য add_action হুক যুক্ত করে কলব্যাক ফাংশন লিখব, তখন অপশন টেবিলে category_tips_and_tricks_counter নামে একটি নতুন অপশন এন্ট্রি (row) যুক্ত হবে। যত বার Tips and Tricks ক্যাটাগরী পেজটি ভিউ হবে, তত বার ঐ ক্যাটাগরীর কাউন্টার এক এক করে বাড়তে থাকবে।

এটা করতে হলে নিচের মত করে কোড লিখতে পারি:

```php
do_action( "philosophy_category_page", single_cat_title( '', false ) );
```

আর functions.php ফাইলের কোড:

```php
function beginning_category_page( $category_title ) {
	if( "Tips and Tricks" == $category_title ) {
		$visit_count = get_option("category_tips_and_tricks_counter" );
		$visit_count = $visit_count ? $visit_count : 0;
		$visit_count++;

		// write to alp_options table
		update_option( "category_tips_and_tricks_counter", $visit_count );
	}
}
add_action( "philosophy_category_page", "beginning_category_page");
```

## ১৯.২ - ফিল্টার হুক এবং হুকের প্যারামিটার নিয়ে বিস্তারিত

থিমের কোন স্থানে ফিল্টার হুক যুক্ত করতে হলে **add_filters( "filter_name", "text_to_display_and_modify" );** ব্যবহার করতে হবে। ফিল্টার সব সময় ডাটা গ্রহণ করে। এই ফিল্টারকে কাস্টমাইজ করতে হলে functions.php ফাইলে **add_filter("filter_name", "callback_function_name" );** যুক্ত করতে হবে।;

থিমের গুরুত্বপূর্ণ প্যারামিটারের ক্ষেত্রে ফিল্টার হুক লিখে রাখা ভাল। তাহলে ক্লায়েন্টের জন্য থিম কাস্টমাইজ করার সুবিধা হয়। আর, ক্লায়েন্ট থিম পছন্দ করলে **সেল বাড়বে**।
