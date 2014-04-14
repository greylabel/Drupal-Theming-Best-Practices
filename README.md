theme-bp
========

#TOC
- [Purpose](http://github.com/torq/theme-bp#purpose)
- [Base Theme](http://github.com/torq/theme-bp#base-theme)

##Purpose
We have a lot more bandwidth now, and browsers are better now, than 20 years ago. However, sites have gotten a lot more complicated than they were 20 years ago. So in some cases, the following guidelines may seem unnecessary. Even so, when theming a site, every little bit counts, so the following things should be kept in mind.

##Base Theme
Choose a base theme based on the project design. In most cases this calls for a clean unstyled theme that sets up a solid foundation. Two such themes are Zen 5 ( https://drupal.org/project/zen ) & Basic ( https://drupal.org/project/basic ). These themes are HTML5, come with a grid system, use SASS that is organized based on SMACSS guidelines, and have small footprints. Out of those two base themes, we recommend Zen 5, because it also supports the Compass plug-in for SASS.

In most situations a full blown framework as a base theme isn’t recommended. Frameworks such as bootstrap or foundation are great starting points when the site is designed based on the framework. But, when they aren’t, they can add over 100kb & 6000 lines of CSS to a project that is mostly being overwritten or largely unused.
