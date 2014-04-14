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

https://docs.google.com/a/acquia.com/document/d/1f7_nQycpHcGu9qEnJUTOyGe-5882jbm4P7gbo_do5i8/edit?usp=sharing
