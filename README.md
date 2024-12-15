# WordPress Froala WYSIWYG Editor


## Compatibility

Version: 3.3.0 up to present versions of WordPress.


## Manual Installation

Clone or [download the contents of this repo](https://github.com/froala/wordpress-froala-wysiwyg/archive/master.zip), make a new folder inside your WordPress installation under plugins folder.
Copy the contents that you previously downloaded to the new folder.

The plugin will be available under plugins in your WordPress admin area.

## Installation

Enter your admin area of your WordPress installation, go to plugins and click add new. Search for Froala WYSIWYG Editor
and follow the automated process. In most cases the "ftp://" credentials will be needed. This is default WordPress behaviour
when installing new plugins.

## Integration

Go to your plugins page inside the admin area of your WordPress installation and activate the plugin.
**The plugin will replace the default editor**.


## Usage

The plugin can be used under the admin area as soon as it is active.

The plugin has a settings page that will be available after you activate the plugin. Under the settings page there will 
be an input for the licence key and a dropdown with all the available plugins that can be activated/deactivated.

To use the Froala Editor on the front-end part of the website, the plugin must be initialized from themes folder.

The activate function accepts 2 parameters but the editor can be init using just one. The second param can be an array or object of options
that can be passed to the editor.

**For a complete list of options have a look over our [options list](https://www.froala.com/wysiwyg-editor/docs/options).**
#### Constants:

```php
define('FroalaCustomJSFolderPath', '/'.basename(__DIR__).'/custom/js');
define('FroalaCustomCSSFolderPath', '/'.basename(__DIR__).'/custom/css');

```

#### Public hooks:

```php

// There are 2 available hooks that work for the front-end part of the website.
// froala_before_public_init acts before the editor gets initialized and 
// froala_after_public_init acts after the editor and all the plugins are loaded.
// Callback function for these hooks accepts 4 params

/** Callback function for public hooks"
 *
 * @param null $path        * File path on server.
 * @param null $type        * Can be js or css
 * @param string $prop      * Can be inline|file
 * @param null $mix         * If prop = file, mix will be the file name else if prop = inline mix will be the data.
 *
 * @return array|WP_Error
 *
 *
* To use a public hook, it needs to be registered right after the editor get's instantiated. The proper way 
* would be to store it in a variable so you can have access to the debug log.
*
* This example includes a custom css file and loads it accordingly, because it's used after public init the css file
* will be at the very bottom of your head tag.

* To understand better, the params are in this way: 
* 1' st froala_after_public_init        => name of the hook.
* 2' nd $custom_css_path.'/test.css'    => path to the file.
* 3' rd 'css'                           => script type.
* 4' th 'file'                          => script property, can be file|inline.
* 5' th 'test'                          => the name of the file. 
*/
$custom_css_path = plugins_url(FroalaEditorCustomCSSFolderPath);
$custom_js_path = plugins_url(FroalaEditorCustomJSFolderPath);

$hook = apply_filters('froala_after_public_init', $custom_css_path.'/test.css', 'css', 'file','test');

if( is_wp_error( $hook ) ) {
	echo $hook->get_error_message();
}

// Same as the example above but it includes a javascript file and the action of the hook it's before Froala Editor's initialization.
$hook = apply_filters('froala_before_public_init', $custom_js_path.'/test.js', 'js', 'file','test');

if( is_wp_error( $hook ) ) {
  echo $hook->get_error_message();
}
// Example using inline script

$hook = apply_filters('froala_after_public_init', null, 'js', 'inline', 'console.log("test")');

if( is_wp_error( $hook ) ) {
  echo $hook->get_error_message();
}

// Example using inline css
$hook = apply_filters('froala_before_public_init', null, 'css', 'inline', 'h1 {background-color: #00ffff;}');


if( is_wp_error( $hook ) ) {
  echo $hook->get_error_message();
}

// Note!! 
//The hooks must be registered right after instantiating the FroalaEditor class.

$Froala_Editor = new Froala_Editor();
.
.
.
$hook = apply_filters('froala_before_public_init', null, 'css', 'inline', 'h1 {background-color: #00ffff;}');
.
.
$froala->activate('#comment',array('colorsBackground   '=> ['#61BD6D', '#1ABC9C', '#54ACD2', 'REMOVE'],
                                         'colorsText'         => ['#61BD6D', '#1ABC9C', '#54ACD2', 'REMOVE']
                                        ));
```

#### Admin hooks:

```php

// There are 2 available hooks that work for the admin part of the website.
// froala_before_init acts before the editor gets initialized and 
// froala_after_init acts after the editor and all the plugins are loaded.
// Callback function for these hooks accepts 4 params

/** Callback function for public hooks"
 *
 * @param null $path        * File path on server.
 * @param null $type        * Can be js or css
 * @param string $prop      * Can be inline|file
 * @param null $mix         * If prop = file, mix will be the file name else if prop = inline mix will be the data.
 *
 * @return array|WP_Error
 *
 *
* To use a private hook, it needs to be registered before the editor get's initialized. The proper way 
* would be to store it in a variable so you can have access to the debug log.
*
* This example includes a custom css file and loads it accordingly, because it's used after admin init the css file
* will be at the very bottom of your head tag.

* To understand better, the params are in this way: 
* 1' st froala_after_public_init        => name of the hook.
* 2' nd $custom_css_path.'/test.css'    => path to the file.
* 3' rd 'css'                           => script type.
* 4' th 'file'                          => script property, can be file|inline.
* 5' th 'test'                          => the name of the file. 
*/

$custom_css_path = plugins_url(FroalaEditorCustomCSSFolderPath);
$custom_js_path = plugins_url(FroalaEditorCustomJSFolderPath);

$hook = apply_filters('froala_after_init', $custom_css_path.'/test.css', 'css', 'file','test');

if( is_wp_error( $hook ) ) {
  echo $hook->get_error_message();
}
// Same as the example above but it includes a javascript file and the action of the hook it's before Froala Editor's initialization.

$hook = apply_filters('froala_before_init', $custom_js_path.'/test.js', 'js', 'file','test');

if( is_wp_error( $hook ) ) {
  echo $hook->get_error_message();
}
// Example using inline script

$hook = apply_filters('froala_after_init', null, 'js', 'inline', 'console.log("test")');

if( is_wp_error( $hook ) ) {
  echo $hook->get_error_message();
}
// Example using inline css

$hook = apply_filters('froala_before_init', null, 'css', 'inline', 'h1 {background-color: #00ffff;}');

if( is_wp_error( $hook ) ) {
 echo $hook->get_error_message();
}

```


#### Example of simple init:

```php
// Public method for easy instantiation for the editor.
// '#comment'  Represents the html element selector.

$Froala_Editor = new Froala_Editor();
$Froala_Editor->activate('#comment');

```

#### Example of initializing using 2 params:

```php

// Static method for easy instantiation for the editor.
// '#comment'  Represents the html element selector.
// 'array()'   Represents the list of options that are passed to the editor.

$Froala_Editor = new Froala_Editor();
$Froala_Editor->activate('#comment',array('colorsBackground' => ['#61BD6D', '#1ABC9C', '#54ACD2', 'REMOVE'],
                                         'colorsText'       => ['#61BD6D', '#1ABC9C', '#54ACD2', 'REMOVE']
                                        ));
                                        
```

#### Example for usage on the front-end but saving the images inside WordPress media library:

```php

// Public method for easy instantiation of the Froala WYSIWYG
// '#comment'  Represents the html element selector.
// 'array()'   Represents the list of options that are passed to the editor.

$Froala_Editor = new Froala_Editor();
$Froala_Editor->activate('#comment',array(
                                    'imageUploadParams'  => ['action' =>'froala_upload_files'],
                                    'imageUploadURL'     => admin_url( 'admin-ajax.php' ),
                                    'imageManagerLoadParams'   => ['action' =>'froala_image_manager'],
                                    'imageManagerLoadURL'=> admin_url( 'admin-ajax.php' )
                                    ));

```

#### Example for adding new plugin for Froala Editor:
This will be visible in the admin under Froala WYSIWYG settings and can be activated/deactivated

```php

// FroalaEditorCustomJSFolderPath is a constant defined on plugin activation will return
// the path to the Custom JS folder e.g: /froala/custom/js    
// froala_new_plugin, custom hook that integrates the new plugin and registers the new script
// The hook takes 2 params, 1'st the path to the plugin and 2'nd the name of the plugin.
// The if statement check if there are any errors on registering the new plugin

$custom_plugin_path = plugins_url(FroalaEditorCustomJSFolderPath);
$new_plugin = apply_filters('froala_new_plugin', $custom_plugin_path . '/test.js', 'test');

if( is_wp_error( $new_plugin ) ) {
	echo $new_plugin->get_error_message();
}

After calling the hook inside the admin panel under Froala WYSIWYG settings there will be a new plugin in the list
called "test".

```
Add the above code to froala.php file to add plugin inside admin panel. After adding a new plugin, it needs to be activated from the admin panel. For an easier understanding the plugin will come with a dummy file placed inside "froala/custom/js/". You can delete this file at any time it's just for demo purposes.

For checking out how plugin works without adding it to admin panel, use the following code inside functions file in your theme:

```php
$custom_plugin_path = plugins_url(FroalaEditorCustomJSFolderPath);
$new_plugin = apply_filters('froala_before_public_init', $custom_plugin_path . '/test.js','js','file', 'test');

if( is_wp_error( $new_plugin ) ) {
  echo $new_plugin->get_error_message();
}

```
This will help if you copy/paste the above code inside your functions file.
 


## License

The `WordPress Froala WYSIWYG Editor` project is under MIT license. However, in order to use WordPress Froala WYSIWYG Editor plugin you should purchase a license for it.

Froala Editor has [3 different licenses](https://www.froala.com/wysiwyg-editor/pricing) for commercial use. For details please see [License Agreement](https://www.froala.com/wysiwyg-editor/terms).

