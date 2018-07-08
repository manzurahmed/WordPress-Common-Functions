## ১৩ - থিম রিকোয়ারমেন্টস (সিকিউরিটি)

### ১৩.১ - ইন্ট্রোডাকশন এবং ভ্যালিডেশন

### ১৩.২ - স্যানিটাইজেশন

https://codex.wordpress.org/Validating_Sanitizing_and_Escaping_User_Data

### ১৩.৩ - ডেটাবেজ

থিম থেকে কখনই ডেটাবেজ এর সাথে সরাসরি ইন্টারএ্যাক্ট করা উচিত নয়, এটা প্লাগইন টেরিটোরিরর কাজ।

যদি কাজ করতেই হয়, সেক্ষেত্রে ওয়ার্ডপ্রেসের wpdb ক্লাস ব্যবহার করতে হবে।

### ১৩.৪ - এসকেপিং আউটপুট

### ১৩.৫ - ক্যাপাবিলিটিজ

current_user_can() ব্যবহার করতে হবে।

https://developer.wordpress.org/plugins/security/checking-user-capabilities/

### ১৩.৬ - নন্স

https://developer.wordpress.org/plugins/security/nonces/

Must reading on Nonces:

1. Using Nonces to Strengthen WordPress Security: https://premium.wpmudev.org/blog/wordpress-nonces/
2. What Are WordPress Nonces? https://www.sitepoint.com/what-are-wordpress-nonces/

### ১৩.৭ - এসভিজি এবং চেঞ্জলগ

থিম থেকে ব্যবহারকারীদেরকে কখনই SVG আপলোড করতে দেয়া যাবে না।
