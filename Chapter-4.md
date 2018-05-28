# Class 4.2
## কাস্টম ফিল্ডের সাথে পরিচয়

```
get_post_meta( int $post_id, string $key = '', bool $single = false )
```
Retrieve post meta field for a post.

Return: (mixed) Will be an array if $single is false. Will be value of meta data field if $single is true.

```php
get_post_meta( get_the_ID(), "placeholder", true );
```

# Class 4.3
## ওয়ার্ডপ্রেসে পিএইচপি স্ক্রিপ্ট থেকে জাভাস্ক্রিপ্টের কাছে ডেটা পাঠানো 

functions.php ফাইলে wp_enqueue_scripts হুকের কলব্যাক ফাংশনে নীচের কোড দিয়ে জাভাস্ক্রিপ্ট এর কাছে ভ্যালু পাস করা যায়।

```php
wp_enqueue_script( '**main-jquery**', get_theme_file_uri("/assets/js/main.js"), array('jquery'), null, true );

$launcher_year = get_post_meta( get_the_ID(), 'year', true );
$launcher_month = get_post_meta( get_the_ID(), 'month', true );
$launcher_day = get_post_meta( get_the_ID(), 'day', true );

 // wp_localize_script( $handle, $name, $data );

wp_localize_script( '**main-jquery**', 'datedata',
  array(
    "year" => $launcher_year,
    "month" => $launcher_month,
    "day" => $launcher_day
  )
);
```
**গুরুত্বপূর্ণ নোট**

যে main.js ফাইলকে যে handle দিয়ে নামকরণ করা হয়েছে, সেই একই handler এর নাম wp_localize_script এ ব্যবহার করতে হবে। তা নইলে ডাটা পাস হবে না।

এই ডাটাটা পাস করা হয়েছে main.js ফাইলে। জেএস ফাইলটি datedata নামে একটি অবজেক্ট আকারে ডাটাটা পাবে, যার মধ্যে ৩টি প্রপার্টি থাকবে, year, month, এবং day

