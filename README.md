Shareable
=========
<img src="https://cloud.githubusercontent.com/assets/28847/3418917/8883ea2e-fe51-11e3-81f4-368f184b9a49.png" title="Shareable makes social media buttons." /><br/>
A gem for adding social buttons to your Rails app. Shareable runs with zero configuration<sup>*</sup> and offers a wide range of customization features. Rendering javascript buttons and static links is easy. One method call can generate all the sharing buttons you want. The available social media sites include:
  - Facebook
  - Twitter
  - Pinterest
  - Reddit
  - Linkedin
  - Google Plus
  - Tumblr<sup>**</sup>
  - StumbleUpon<sup>**</sup>

The Shareable gem supports Rails 3 and 4.

<sub>* If you're using Turbolinks, set configuration option *turbolinks* to **true** (see [section on Turbolinks](https://github.com/hermango/shareable#turbolinks)).</sub><sub> ** Tumblr and StumbleUpon buttons aren't outputted by default. Set a [configuration](https://github.com/hermango/shareable#configuration) option to show these buttons.</sub>


Install
--------
Put this line in your Gemfile:

  gem 'shareable'

Then run the bundle command:

  bundle

Usage
------
### Views

For the simplest way of using Shareable, add this helper method to your views:

```no-highlight
<%= render_shareable %>
```

That's it! You can change the look and content of each button with more options. Keep reading to find out how.

You can also render each social media button separately by calling the following methods:
+ facebook_button
+ twitter_button
+ pinterest_button
+ linkedin_button
+ reddit_button
+ google_plus_button
+ tumblr_button
+ stumble_upon_button

#### Static Links

Shareable generates javascript buttons by default. To render plain html links instead, set the configuration option *static_link* to boolean value **true** in your app's local 'shareable.rb' configuration file. You can also pass *static_link* to the render_shareable method like so:

```no-highlight
<%= render_shareable :static_link => true %>
```

#### Tumblr and StumbleUpon Buttons

By default the display of Tumblr and StumbleUpon buttons is disabled. Enable either button by including the name of the button you want to add (**tumblr** or **stumble_upon**) to the configuration option *names* (found in your local 'shareable.rb' configuration file). [See Configuration](https://github.com/hermango/shareable#configuration) for more information. 


### Advanced Usage
Pass hash values to the render_shareable method to overwrite options locally:
```no-highlight
<%= render_shareable :url=> 'http://github.com/hermango/', :facebook=> {:send=> 'true', :url=> 'http://demo.dev'} %>
```

To render only certain buttons:
```no-highlight
<%= render_shareable :buttons=> ['twitter', 'facebook', 'tumblr'] %>
```

Alternatively, omit the render_shareable method entirely and instead add each social link individually.
The helper method for each social link is the site name joined with an underscore to the word 'button'. E.g.:

```no-highlight
<%= twitter_button %>
<%= reddit_button :title=> 'Alternate Title' %>
```

#### Customizing Buttons 
Overriding Shareable is possible and entirely optional. Some knowledge of [Rails layouts and rendering](http://guides.rubyonrails.org/layouts_and_rendering.html) is required. Read on to learn how you can personalize your buttons.

Buttons and static links are wrapped in overwritable files called partials. The look and DOM structure of your buttons can be changed by creating your own files with matching filenames and placed in your local 'views' directory. As long as the yield method is called in the following two partials, you can make whatever changes you want:
+ [_parent_wrapper.html.erb](https://github.com/hermango/shareable/blob/master/app/views/shareable/partials/_parent_wrapper.html.erb) goes in the 'app/views/shareable/partials/' directory. This partial is rendered once and encloses all of the buttons.
+ [_button_wrapper.html.erb](https://github.com/hermango/shareable/blob/master/app/views/shareable/partials/_button_wrapper.html.erb) goes in the 'app/views/shareable/partials/' directory. This partial is rendered inside _parent_wrapper.html.erb and is outputted for each button.

The partial used for static links is named _link.html.erb. The important variable you want to include is called *options[:url]*.
+ [_link.html.erb](https://github.com/hermango/shareable/blob/master/app/views/shareable/partials/_link.html.erb) goes in the 'app/views/shareable/partials/' directory.

To override the actual code for a button, place your own template file in the
'app/views/shareable/' directory of your app and prepend that filename with an underscore and the name of the social media site. For example, to overwrite the Facebook button: Create a file called _facebook.html.erb that goes in the 'app/views/shareable/' directory.

##### button_name and button_caption
Each outputted button has two self-referencing variables that are accessible to you. These variables are named *button_name* and *button_caption* and are contained in the *options* hash. Depending on the button instance these variables are accessed within, the possible string values are:
+ options[:button_name] - facebook, twitter, pinterest, linkedin, google_plus, reddit, tumblr, stumble_upon 
+ options[:button_caption] - Facebook, Twitter, Pinterest, Linkedin, Google+, Reddit, Tumblr, StumbleUpon

Both variables can be overridden when passed to the render_shareable method as hash values. E.g.:
```
<%= render_shareable :twitter => {:button_caption => 'Tweet this!'} %>
```

#### Facebook compatibility with Internet Explorer
Add an XML namespace fb='http://ogp.me/ns/fb#' to the html tag of your view. Eg: 
```no-highlight
<html xmlns:fb="http://ogp.me/ns/fb#">
```

This is necessary for XFBML to work in earlier versions of Internet Explorer.

#### Styling for social buttons
For horizontally aligned buttons, try this CSS:

```no-highlight
nav.share {display:inline-block;} /* shareable's nav tag */
nav.share div {float:left; overflow:hidden;}
nav.share iframe {max-height:20px; border:0; margin-top:0; padding-top:0;} /* if using medium sized buttons */
#___plusone_0 {max-width:70px; font-size: default !important;}
.twitter-share-button, .twitter-count-horizontal {max-width:80px;}
.fb_iframe_widget span { vertical-align: top; }
.fb_edge_widget_with_comment { vertical-align: top;}
.fb-like{display:inline-block}
* html .fb-like{display:inline}/* ie6 inline block fix*/
*+html .fb-like{display:inline}/* ie7 inline block fix*/
```

If you use the default configuration options and styling above, the output should appear as:
<img src="https://cloud.githubusercontent.com/assets/28847/3418917/8883ea2e-fe51-11e3-81f4-368f184b9a49.png" alt="horizontal row of evenly aligned social media buttons." />

###Turbolinks
Shareable is compatible with Turbolinks but requires the javascript library [jQuery](https://github.com/rails/jquery-rails). Turbolinks support is turned off by default. To enable Turbolinks support, first make sure your app uses jQuery then set configuration option *turbolinks* in your local 'shareable.rb' [configuration](https://github.com/hermango/shareable/blob/master/README.md#configuration) file to boolean value **true**. The *turbolinks* option can also be passed as a hash value to the render_shareable method like this:

```
<%= render_shareable :turbolinks => true, :facebook => {:turbolinks => false} %>
```
If your app doesn't use jQuery or you don't want to use the provided solution but still want to use this gem, here are some other options:

 1. Use [static links](https://github.com/hermango/shareable#static-links) instead.
 2. Roll your own solution: Set configuration option *button_only* to boolean value **true**. This will output button tags without javascript. Then you can add your own javascript solution. See [Nick Reed's tutorial](http://reed.github.io/turbolinks-compatibility/) to check out some approaches.
 3. Disable Turbolinks. To disable Turbolinks from your app: Remove the line "//= require turbolinks" from your app's application.js file in the assets directory. To disable Turbolinks in some pages: use the data marker *data-no-turbolink* in your links and html elements [(instructions here)](https://github.com/rails/turbolinks#opting-out-of-turbolinks).

Configuration
------------
This gem comes pre-configured and ready to use. You can use the [default configuration settings](https://github.com/hermango/shareable/blob/master/lib/shareable/config.rb) or customize your buttons by adding and editing your own initializer named 'shareable.rb' to your app's 'config/initializers' directory.

A generator is available for generating the default configuration file into your project's 'config/initializers' directory.
Run the following command, then edit the generated file as preferred.

rails g shareable:config

String values are expected for nearly all of the configuration options. Any values passed as configuration options not enclosed in quotation marks will cause errors! Four configuration options are exceptions to this rule. The configuration option *names* (also called *buttons* when passed to the render_shareable helper method) expects an array of strings ([see the configuration file for more details](https://github.com/hermango/shareable/blob/master/lib/shareable/config.rb)). The other three configuration options expect boolean values only and are the following:
 * static_link - **false** by default. See [Static Links](https://github.com/hermango/shareable#static-links).
 * button_only - **false** by default. If set to **true**, javascript code for buttons is omitted.
 * turbolinks - **false** by default. When set to **true**, social buttons will work with Turbolinks. Relies on jQuery.

---

This project was inspired by Akira Matsuda's Kaminari project and uses the MIT-LICENSE. Suggestions, bug reports, criticism and all contributions welcome.
