theme-bp
========

#TOC
- [Purpose](http://github.com/torq/theme-bp#purpose)
- [Base Theme](http://github.com/torq/theme-bp#base-theme)
- [Grid System](http://github.com/torq/theme-bp#grid-system)
- [Style Organization](http://github.com/torq/theme-bp#style-organization)

##Purpose
We have a lot more bandwidth now, and browsers are better now, than 20 years ago. However, sites have gotten a lot more complicated than they were 20 years ago. So in some cases, the following guidelines may seem unnecessary. Even so, when theming a site, every little bit counts, so the following things should be kept in mind.

##Base Theme
Choose a base theme based on the project design. In most cases this calls for a clean unstyled theme that sets up a solid foundation. Two such themes are Zen 5 ( https://drupal.org/project/zen ) & Basic ( https://drupal.org/project/basic ). These themes are HTML5, come with a grid system, use SASS that is organized based on SMACSS guidelines, and have small footprints. Out of those two base themes, we recommend Zen 5, because it also supports the Compass plug-in for SASS.

In most situations a full blown framework as a base theme isn’t recommended. Frameworks such as bootstrap or foundation are great starting points when the site is designed based on the framework. But, when they aren’t, they can add over 100kb & 6000 lines of CSS to a project that is mostly being overwritten or largely unused.

##Grid System
When creating a theme you can be tempted to not use a grid system, or to create your own grid system. However, this technique breaks down when there are multiple themers and in many multi-language sites. Grid systems and base themes allow the layouts to adapt to RTL language sites. Using and sticking to a grid system provides consistency when there are multiple themers working on multiple parts of a project.

## Style Organization
When organizing styles, group styles based on the SMACSS ( http://smacss.com/ ) guidelines. If using a base theme like Zen 5 or Basic, the SASS directory is organized like this by default. Module Rules are renamed as Component Rules to not confuse themers. It is recommended to add media queries in the relevant component styles and not with the state style rules.

*With SMACSS, the intent is to keep the styles that pertain to a specific component with the rest of the component. That means that instead of having a single break point, either in a main CSS file or in a separate media query style sheet, place media queries around the component states.*

http://www.acquia.com/blog/organize-your-styles-introduction-smacss

##CSS Preprocessing
CSS preprocessors allow themers to be more efficient when developing a sub theme. We prefer SASS, using scss syntax, and using Compass. These are the Zen 5 defaults and work well. We recommend using Compass for creating sprites for your site using two sprite directories, one for standard resolution, one for retina resolutions.

##Style Rules
When writing styles, the themer’s goal should be to write efficient CSS. This means using the least amount of selectors possible. The best performance in CSS is the ID, but this is often not a realistic selector to use. The exceptions are for layout rules and unique rules for unique items, like the site-name. After that is the class selector. Though we write CSS left to right ( `#content .field-item p` ) a browser reads CSS right to left. So in the previous rule, it would first find every paragraph on the page. Then it invalidate the ones that aren’t inside a `.field-item class`. Then invalidate the remaining ones that aren’t inside of an element with the ID #content. When using a CSS Preprocessor, a lot of care needs to be taken in regards to selector depth. It’s extremely easy to nest selectors which will result in extremely inefficient styles.

Drupal can be quite a challenge. And in most cases, you need to style elements that aren’t IDs. In most cases you can, and should, use classes. Keep your styles generic when you can, think broad strokes. If you can, apply your style to `.field-item` instead of `.article .field-item`. Likewise, when you need to apply the style to a more limited scope, use the semantic class and not the drupal generic class. So, use `.view-articles` instead of simply `.view`.

**Example: You need to apply a style to an ul, li, and an a tag for a particular view of articles. You may be tempted to write SASS like this:**

`
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
`