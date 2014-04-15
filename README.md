theme-bp
========

#TOC
- [Purpose](http://github.com/torq/theme-bp#purpose)
- [Base Theme](http://github.com/torq/theme-bp#base-theme)
- [Grid System](http://github.com/torq/theme-bp#grid-system)
- [Style Organization](http://github.com/torq/theme-bp#style-organization)
- [CSS Preprocessing](http://github.com/torq/theme-bp#css-preprocessing)
- [Style Rules](http://github.com/torq/theme-bp#style-rules)
- [Template Files](http://github.com/torq/theme-bp#template-files)
- [Working with Javascript](http://github.com/torq/theme-bp#working-with-javascript)
- [Links](http://github.com/torq/theme-bp#links)

##Purpose
We have a lot more bandwidth now, and browsers are better now, than 20 years ago. However, sites have gotten a lot more complicated than they were 20 years ago. So in some cases, the following guidelines may seem unnecessary. Even so, when theming a site, every little bit counts, so the following things should be kept in mind.

##Base Theme
Choose a base theme based on the project design. In most cases this calls for a clean unstyled theme that sets up a solid foundation. Two such themes are Zen 5 ( https://drupal.org/project/zen ) & Basic ( https://drupal.org/project/basic ). These themes are HTML5, come with a grid system, use SASS that is organized based on SMACSS guidelines, and have small footprints. Out of those two base themes, we recommend Zen 5, because it also supports the Compass plug-in for SASS.

In most situations a full blown framework as a base theme isn’t recommended. Frameworks such as bootstrap or foundation are great starting points when the site is designed based on the framework. But, when they aren’t, they can add over 100kb & 6000 lines of CSS to a project that is mostly being overwritten or largely unused.

##Grid System
When creating a theme you can be tempted to not use a grid system, or to create your own grid system. However, this technique breaks down when there are multiple themers and in many multi-language sites. Grid systems and base themes allow the layouts to adapt to RTL language sites. Using and sticking to a grid system provides consistency when there are multiple themers working on multiple parts of a project.

##Styles

### Style Organization
When organizing styles, group styles based on the SMACSS ( http://smacss.com/ ) guidelines. If using a base theme like Zen 5 or Basic, the SASS directory is organized like this by default. Rules are broken up into 5 main areas.
- **Base Rules** > Bases styles for HTML elements and normalization rules
- **Layout Rules** > Styles setting up widths and placement of regions 
- **Component Rules** > The majority of your rules. Should be in a partials directory. Partials should be semantically named for the component they apply to.
- **State Rules** > These are often utility styles applied or toggled by javascript. Quicktabs, collapsible sections, show/hide.
- **Theme Rules** > These are theme styles for things like page background, typography, colors, etc.

Component rules are commonly referred to as *module rules* when discussing SMACSS, however, the term *module styles* means something completely different when dealing with Drupal. It is recommended to add media queries in the relevant component styles and not with the state style rules.

*With SMACSS, the intent is to keep the styles that pertain to a specific component with the rest of the component. That means that instead of having a single break point, either in a main CSS file or in a separate media query style sheet, place media queries around the component states.*

###CSS Preprocessing
CSS preprocessors allow themers to be more efficient when developing a sub theme. We prefer SASS, using scss syntax, and using Compass. These are the Zen 5 defaults and work well. We recommend using Compass for creating sprites for your site using two sprite directories, one for standard resolution, one for retina resolutions.

###Style Rules
When writing styles, the themer’s goal should be to write efficient CSS. This means using the least amount of selectors possible. The best performance in CSS is the ID, but this is often not a realistic selector to use. The exceptions are for layout rules and unique rules for unique items, like the site-name. After that is the class selector. Though we write CSS left to right ( `#content .field-item p` ) a browser reads CSS right to left. So in the previous rule, it would first find every paragraph on the page. Then it invalidate the ones that aren’t inside a `.field-item class`. Then invalidate the remaining ones that aren’t inside of an element with the ID #content. When using a CSS Preprocessor, a lot of care needs to be taken in regards to selector depth. It’s extremely easy to nest selectors which will result in extremely inefficient styles.

Drupal can be quite a challenge. And in most cases, you need to style elements that aren’t IDs. In most cases you can, and should, use classes. Keep your styles generic when you can, think broad strokes. If you can, apply your style to `.field-item` instead of `.article .field-item`. Likewise, when you need to apply the style to a more limited scope, use the semantic class and not the drupal generic class. So, use `.view-articles` instead of simply `.view`.

**Example: You need to apply a style to an ul, li, and an a tag for a particular view of articles. You may be tempted to write SASS like this:**

```
.article-view {
  /* view styles */
  ul {
    /* ul styles */
    li {
      /* li styles */
      a {
        /* a styles */
      }
    }
  }
}
```
**The previous would compile to:**
```
.article-view {/* view styles */}
.article-view ul {/* ul styles */}
.article-view ul li {/* li styles */}
.article-view ul li a {/* a styles */}
```
**A better way would be:**
```
.article-view {
  /* view styles */
  ul { */ ul styles */}
  li { /* li styles */}
  a {/* a styles */}
}
```
**Which compiles to:**
```
.article-view {/* view styles */}
.article-view ul {/* ul styles */}
.article-view li {/* li styles */}
.article-view a {/* a styles */}
```
If you look at the first set (the bad one) in the previous example, the last rule requires the browser match 4 items. Since a browser reads right to left, this is extremely inefficient. Nested tags is the most expensive rule in regards to front-end performance. If the HTML tags have classes, like a typical drupal site would, the most efficient rule would use those.

For more information on CSS selector efficiency: https://developers.google.com/speed/docs/best-practices/rendering

If you find yourself fighting a module’s, or drupal core’s default stylesheet and need to remove it’s css file, you should use a hook_css_alter() to unset the file in your template.php file in your theme directory.

A great module that can be used to set up your theme’s generic styles is the style guide module ( https://drupal.org/project/styleguide ).

Though drupal.org style guidelines don’t consider css preprocessors very much, and some information seems outdated, the css guidelines are worth reading. https://drupal.org/node/1886770

##Template Files
HMTL and your php variables go in the tpl files. Logic goes into preprocess and process functions that are located in the template.php file. The most important part of working with template files is being consistent with your mark up. If you’d like to clean up some of the standard drupal mark up, you can use the Fences module ( https://drupal.org/project/fences ) to provide a leaner structure.

A good general rule is, if you can accomplish a task with a preprocessor function or adding a tpl file, go with the preprocessor function because it's more efficient. A example preprocessor function to add a class to a block title. Here's the source of my block when I view it using chrome dev tools.
```
<div id="block-block-1" class="block block-block first last odd">
  <h2 class="block__title block-title">Head Block Demo</h2>
  <p>some stuff</p>
</div>
```
In this example, my theme name is `torq`, the block delta is 1, which I pull from the source (`id=block-block-1`). Here's my simple preprocessor:

```
/**
 * Override or insert variables into the block templates.
 *
 * @param $variables
 *   An array of variables to pass to the theme template.
 * @param $hook
 *   The name of the template being rendered ("block" in this case.)
 */

function torq_preprocess_block(&$variables, $hook) {

  $block = $variables['block'];

  if ($block->delta == 1) {
    $variables['title_attributes_array']['class'][] = 'awesome-class';
  }
}
```
This example results in:
```
<div id="block-block-1" class="block block-block first last odd">
  <h2 class="block__title block-title awesome-class">Head Block Demo</h2>
  <p>some stuff</p>
</div>
```

Avoid using drupal_add_css() & drupal_add_js() in your template files (and custom modules). It’s better to work your styles into your theme. It can be tempting to use the conditional methods of adding css & js as a way to keep file sizes down on page loads. But it also causes Drupal to create multiple versions of the aggregated css & js. One file gets downloaded once and cached locally. In extreme cases, where drupal_add is being used a lot, drupal can create a new aggregate on nearly every page load. CSS & JS aggregate files are normally cached heavily by varnish. Creating new aggregates, however, circumvents the caching, and creates server load from not only having to continuously serve these files, but continuously creating these files.

If the styles are added within the normal site styles, they are simply aggregated once, cached by varnish, downloaded on the first page load, and cached locally.
- Template Suggestions: https://drupal.org/node/223440
- Theme Hook Suggestions: https://drupal.org/node/1089656
- A list of the core tpl files can be found here: https://drupal.org/node/190815

##Working with Javascript
Working with javascript is more difficult than working with styles. Please review the javascript coding standards here: https://drupal.org/node/172169 which provides information on code structure (indeting, comments, etc.).

It’s usually helpful to create a single site-wide javascript file in your js directory of your theme. Drupal includes jquery (version 1.4.4) in core that can be used. If you need a newer version of jquery then what it ships with, you can use the jQuery Update module ( https://drupal.org/project/jquery_update ). Using jQuery Update you have the ability to choose a different version of the jQuery library.

When you need to utilize a third party javascript library or jQuery plugin, there are multiple ways to accomplish it. One way that seems easy is to simply add the plugin to your theme’s JS folder and add it to your theme’s .info file or using drupal_add_js() to include the plugin. However, the best way is to utilize a plugin or library is the libraries api ( https://drupal.org/project/libraries ). You can add the library and use a custom module to add the library to your site.

Drupal Behaviors are Drupal’s way to fire javascript on document.ready and again after ajax calls. Most of the times, this is what you would want because of things being added to the DOM by ajax after the page is loaded.
```
Drupal.behaviors.yourCustomBehavior = {
  attach: function (context, settings) {
    $(‘#block-block-1’).click(function() {
      // do something
    });
  }
}
```
For things that for some reason shouldn’t be inside of a drupal behavior it should be wrapped in:
```
(function ($) {
  // your js here
}(jQuery));
```





##Links
###Themes
- [Zen5](https://drupal.org/project/zen)
- [Basic](https://drupal.org/project/basic)
- [SMACSS in themes](http://www.acquia.com/blog/organize-your-styles-introduction-smacss)
- [SMACSS](http://smacss.com/)

###Grids
- [Zen Grids](http://zengrids.com/help/)

###Compass
- [Compass](http://compass-style.org/)

###CSS Performance
- [Chrome - Optimize Browser Rendering](https://developers.google.com/speed/docs/best-practices/rendering)
- [Mozilla - Writing Efficient CSS](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Writing_efficient_CSS)

###Drupal Coding Standards
- [CSS](https://drupal.org/node/1886770)
- [JavaScript](https://drupal.org/node/172169)

###Drupal Guides
- [Template Suggestions](https://drupal.org/node/223440)
- [Theme Hook Suggestions](https://drupal.org/node/1089656)
- [Core Tpl Files](https://drupal.org/node/190815)
- [Working with Javascript](https://drupal.org/node/121997)

###Helpful Drupal Modules
- [Fences](https://drupal.org/project/fences)
- [jQuery Update](https://drupal.org/project/jquery_update)
- [Libraries API](https://drupal.org/project/libraries)
- [Style Gide](https://drupal.org/project/styleguide)
