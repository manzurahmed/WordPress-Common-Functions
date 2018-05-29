# Chapter 5

## ৫.১ সিঙ্গেল পোস্টের পেজিনেশন

একটা সিঙ্গেল **বড় পোস্ট** কে একাধিক পেজে বিভক্ত করতে পোস্টের মধ্যে Text মুড এ গিয়ে যেখানে যেখানে পেজ তৈরী করা দরকার, সেখানে সেখানে
```html
<!--nextpage-->
```
লিখতে হবে। এবার single.php ফাইলে এসে the_content(); এর নীচে লিখতে হবে:

```php
wp_link_pages();
```

ব্যস! এই টুকুই যথেষ্ট।

## ৫.২ পোস্ট ফরম্যাট নিয়ে আলোচনা

```php
add_theme_support( 'post-formats', array( 'aside', 'gallery' ) );
```

বিস্তারিত জানতে: https://codex.wordpress.org/Post_Formats

aside, gallery, link, image, quote, status, video, audio, chat

functions.php ফাইলে wp_enqueue_scripts হুকের কলব্যাক ফাংসনে DashIcons এর সিএসএস কে এনকিউ করে নিতে হবে।

```php
wp_enqueue_style( 'dashicons');
```

Dash Icons URL: https://developer.wordpress.org/resource/dashicons/#phone

