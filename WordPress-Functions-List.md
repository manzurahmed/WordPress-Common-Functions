### esc_html__( string $text, string $domain = 'default' )
### esc_html_e( $text, $domain )

Retrieve the translation of $text and escapes it for safe use in HTML output.
```
<h1><?php echo esc_html__( 'Title', 'text-domain' )?></h3>
```

### esc_html( $text )

Escaping for HTML blocks.
```
$html = esc_html( '<a href="http://www.example.com/">A link</a>' );
```
$html now contains this: 
```
&lt;a href=&quot;http://www.example.com/&quot;&gt;A link&lt;/a&gt;
```

### esc_html_e( $text, $domain )

<?php esc_html_e( $text, $domain ); ?>

### esc_attr( string $text )

Escaping for HTML attributes.

```
<input type="text" name="fname" value="<?php echo esc_attr( $fname ); ?>">
```

### esc_url( $url, array $protocols, $_context )

Always use esc_url when sanitizing URLs
```
<a href="<?php echo esc_url( home_url( '/' ) ); ?>">Home</a>
```
