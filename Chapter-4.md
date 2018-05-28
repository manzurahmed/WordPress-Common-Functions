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
