## ৬.১ - ওয়ার্ডপ্রেসে ইমেজ সাইজ নিয়ে বিস্তারিত

ইমেজ সাইজ মূলত: ৩ প্রকারের।

1. স্কয়ার সাইজ
2. প্রোট্রেইট সাইজ
3. ল্যান্ডস্কেপ সাইজ

ওয়ার্ডপ্রেসে ৫টা ডিফল্ট ইমেজ সাইজ থাকে।

1. the_post_thumbnail( 'thumbnail' ); // 150px x 150px
2. the_post_thumbnail( 'medium' ); // 150px x 150px
3. the_post_thumbnail( 'medium_large' ); // 150px x 150px
4. the_post_thumbnail( 'large' ); // 150px x 150px
5. the_post_thumbnail( 'full' ); // 150px x 150px

থিম ফরেস্টে ৭টার বেশি ইমেজ সাইট ব্যবহার করলে, থিম রিজেক্ট করবে।

## ৬.২ - থিমে নতুন ইমেজ সাইজ যোগ করা
```
add_image_size( "alpha-square", width, height, $crop = bool );
add_image_size( "alpha-square", 400, 400, true );
```

শেষের প্যারামিটার দিয়ে ইমেজ ক্রপে ধরণ বোঝায়। true দিয়ে হার্ডক্রপ, আর false দিয়ে সফট ক্রপ বোঝায়।

হার্ডক্রপ = ইমেজকে আর্গুমেন্টে width বা height অনুযায়ী রিসাইজ করবে। রিসাইজ প্রক্রিয়ায় হাইট বা ওয়াডথ যেটা আগে মিট করবে, সেটাকে ঠিক রেখে ইমেজের বাকি অংশ ডিসকার্ড করে দিবে।
সফটক্রপ = width বা height অনুযায়ী রিসাইজ করবে। কিন্তু, কন্ডিশন স্যাটিসফাই করার পর ইমেজের বর্ধিতাংশ ডিসকার্ড করবে না।

## ৬.৩ - নতুন সাইজ যোগ করা হলে পুরনো ইমেজগুলো নিয়ে কি করতে হবে

নতুন ইমেজ সাইট থিমে যুক্ত করার পর "Regenerate Thumbnails" নামে একটি নতুন প্লাগইন ইন্সটল করতে হবে। এই প্লাগইন দিয়ে সাইটের সকল এ্যাটাচমেন্টকে নতুন করে জেনারেট করে নিতে হবে। রিজেনারেট করার পর নতুন যুক্ত ৪টি ইমেজ সাইজ সহ আমার থিমের প্রতিটি এ্যাটাচমেন্টের মোট ৯টি থাম্ব ইমেজ তৈরী হবে।

থিমের ভিতরে নতুন যুক্ত করা ইমেজকে কল করতে হবে ঐ নামে, যে নাম দিয়ে তাদেরকে তৈরী করা হয়েছে।

```php
the_post_thumbnail( 'alpha-square' );
the_post_thumbnail( 'alpha-portrait' );
the_post_thumbnail( 'alpha-landscape' );
the_post_thumbnail( 'alpha-landscape-hard-cropped' );
```

### ৬.৪ - ডিফল্ট ইমেজ সাইজগুলো কখনোই পরিবর্তন করা উচিত নয়

ওয়ার্ডপ্রেস থিমের ডিফল্ট ইমেজ সাইজগুলোকে কখনই পরিবর্তন করা উচিত নয়।

### ৬.৫ - ওয়ার্ডপ্রেসে ইমেজ সাইজের ক্রপ পজিশন নিয়ে বিস্তারিত

add_image_size ফাংশনের মাধ্যমে আমরা ওয়ার্ডপ্রেসকে ইন্সট্রাক্ট করতে পারি কিভাবে ইমেজকে ক্রপ করব। ইমেজ ডাইমেনশনের পরে একটি এ্যারেতে x-axis এবং y-axis উল্লেখ করে এটি করা হয়।

```php
//x-axis: left, center, right
//y-axis: top, center, bottom

add_image_size( "alpha-landscape-hard-cropped", 600, 400, array( "left", "top" ) );
```

ওয়ার্ডপ্রেস **ইমেজের সাইজের** উপরে কাজ করে। যেমন: 400 x 400

ইমেজ সাইজের **হ্যান্ডলের নামের** উপরে নয় যেমন: ("alpha-landscape-hard-cropped")।

### ৬.৬ - ইমেজ সাইজ এবং ক্রপিং এর একটা খুবই ইন্টারেস্টিং প্রবলেম

ভিন্ন ভিন্ন সাইজের ইমেজের জন্য ভিন্ন ভিন্ন ক্রপিং পজিশন বলে দিলাম। কিন্তু, single.php টেমপ্লেটে যখন ভিন্ন ভিন্ন সাইজের ইমেজ দেখাতে যাব, তখন দেখা যায়, শেষের যে ইমেজ সাইজ আছে তার ক্রপিং পজিশন অনুযায়ী ইমেজ দেখায়। এক্ষেত্রে ওয়ার্ডপ্রেস html কোডে **srcset** ব্যবহার করে ইমেজকে রেসপনসিভ করার চেষ্টা করে।

```php
add_image_size( 'alpha-square', 400, 400, true ); // center center
add_image_size( 'alpha-square-new1', 401, 401, array('left', 'top') ); // center center
add_image_size( 'alpha-square-new2', 500, 500, array('center', 'center') );
add_image_size( 'alpha-square-new2', 600, 600, array('right', 'center') );
```

**নোট:** নতুন ইমেজ সাইজ ব্যবহার করার পর Regenerate Thumbnail প্লাগইন দিয়ে ইমেজগুলোকে রিজেনারেট করে নিতে হবে। এটা খুবই রিসোর্স ইনটেনসিভ প্রসেস। সুতরাং, ব্যবহার করার আগে চিন্তাভাবনা করে ব্যবহার করতে হবে।

```php
the_post_thumbnail( 'alpha-square-new1' );
the_post_thumbnail( 'alpha-square-new2' );
the_post_thumbnail( 'alpha-square-new3' );
```

এই ঝামেলা থেকে বাঁচতে wp_calculate_image_srcset হুক ব্যবহার করতে হবে।

```php
function alpha_image_srcset()
{
	return null;
}

add_filter( 'wp_calculate_image_srcset', 'alpha_image_srcset' );
// ওয়ার্ডপ্রেসে "return null" ভ্যালু রিটার্ন করার জন্য আলাদা ইনলাইন ফাংশন আছে, "__return_null"।
// যেখানে null ভ্যালু return করার কথা, সেখানে কলব্যাক ফাংশন ব্যবহার না করে কলব্যাকের জায়গায় "__return_null" ব্যবহার করা যায়।
//add_filter( 'wp_calculate_image_srcset', '__return_null' );
```
