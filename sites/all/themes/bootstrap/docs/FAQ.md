<!-- @file Frequently Asked Questions -->
<!-- @defgroup -->
# FAQ - Frequently Asked Questions

- [Do you support X module?](#support)
- [Do you support Internet Explorer?](#ie)
- [Is Drupal Bootstrap a module or theme?](#module-or-theme)
- [What does the JavaScript error `TypeError: $(...).on is not a function` mean?](#jquery)
- [Where can I discuss an issue in real time?](#irc)
- [Where should I make changes?](#changes)
- [Why are my sub-theme's `.info` settings ignored?](#theme-settings-ignored)

---

### Q: Do you support X module? {#support}
**A: Possibly**

Below are a list of modules the [Drupal Bootstrap] base theme actively supports.
This list is constantly growing and each module's support has usually been
implemented because of either extremely high usage or the fact it was designed
explicitly for use with this base theme and has maintainers in both projects.

**Supported modules:**
- [Admin Menu](https://www.drupal.org/project/admin_menu)
- [Bootstrap Core](https://www.drupal.org/project/bootstrap_core)
- [jQuery Update](https://www.drupal.org/project/jquery_update)
- [Icon API](https://www.drupal.org/project/icon)
- [Path Breadcrumbs](https://www.drupal.org/project/path_breadcrumbs)
- [Picture](https://www.drupal.org/project/picture)
- [Views](https://www.drupal.org/project/picture) _(partial support)_
- [Webform](https://www.drupal.org/project/webform) _(partial support)_

The following modules are "un-supported modules" and are not documented by the
[Drupal Bootstrap] base theme. This does not mean that the base theme will not
work with them or that they are "bad". It simply means that this project does
not have the time, energy or effort it would take to document "every possible
scenario".

It is certainly possible that some of these modules may eventually become
"officially" supported. That will happen only, of course, if there are enough
people to help contribute solid solutions and make supporting them by the base
theme maintainers a relatively "easy" task.

Some of these modules may have blogs or videos floating around on the internet.
However, if you choose to use one of these modules, you are really doing so
at your own expense. Do not expect support from this base theme or the project
you are attempting to integrate the base theme with.

**"Un-supported" modules:**
- Color module (in core)
- [Bootstrap API](https://www.drupal.org/project/bootstrap_api)
- [Bootstrap Library](https://www.drupal.org/project/bootstrap_library)
- [Display Suite](https://www.drupal.org/project/ds)
- [Display Suite Bootstrap Layouts](https://www.drupal.org/project/ds_bootstrap_layouts)
- [LESS module](https://drupal.org/project/less)
- [Panels](https://www.drupal.org/project/panels)
- [Panels Bootstrap Layouts](https://www.drupal.org/project/panels_bootstrap_layouts)

---

### Q: Do you support Internet Explorer? {#ie}
**A: No, not "officially"**

The [Bootstrap Framework] itself does not officially support older Internet
Explorer [compatibility modes](http://getbootstrap.com/getting-started/#support-ie-compatibility-modes).
To ensure you are using the latest rendering mode for IE, consider installing
the [HTML5 Tools](https://drupal.org/project/html5_tools) module.

Internet Explorer 8 requires the use of [Respond.js] to enable media queries
(Responsive Web Design). However, [Respond.js] does not work with CSS that is
referenced via a CSS `@import` statement, which is the default way Drupal
adds CSS files to a page when CSS aggregation is disabled. To ensure
[Respond.js] works properly, enable CSS aggregation at the bottom of:
`admin/config/development/performance`.

---

### Q: Is Drupal Bootstrap a module or theme? {#module-or-theme}
**A: Theme**

More specifically a base theme. It is _not_ a module. Modules are allowed to
participate in certain hooks, while themes cannot. This is a very important
concept to understand and limits themes from participating in a wider range of
functionality.

---

### Q: What does the JavaScript error `TypeError: $(...).on is not a function` mean? {#jquery}
Drupal 7 ships with the `1.4.4` version of jQuery out-of-the-box. The [$.on()](http://api.jquery.com/on/)
method was introduced several versions after this in jQuery version `1.7`.

You must upgrade your site or sub-theme's jQuery Version by installing (and
properly configuring) the [jQuery Update] module to use a jQuery version
compatible with the [Bootstrap Framework]. This is typically a version greater
than or equal to `1.9`.

Ensuring jQuery compatibilities between Drupal, the [Bootstrap Framework] and
other plugins/modules does not fall within the scope of this base theme's
documentation or support.

---

### Q: Where can I discuss an issue in real time? {#irc}
**A: In IRC**

The [Drupal Bootstrap] project and its maintainers use the `#drupal-bootstrap`
channel on the freenode.net IRC network to communicate in real time. Please read
the following for more information on how to the community uses this technology:
[Chat with the Drupal Community on IRC](https://www.drupal.org/irc).

Please keep in mind though, this **IS NOT** a "support" channel. It's primary
use is to discuss issues and to help fix bugs with the base theme itself.

---

### Q: Where should I make changes? {#changes}
**A: In a custom sub-theme**

You should **never** modify any theme or sub-theme that is packaged and released
from Drupal.org. If you do, all changes you have made would be lost once that
theme is updated. This makes keeping track of changes next to impossible.

Instead, you should create a custom sub-theme that isn't hosted on Drupal.org.

[Drupal Bootstrap]: https://www.drupal.org/project/bootstrap
[Bootstrap Framework]: http://getbootstrap.com
[Respond.js]: https://github.com/scottjehl/Respond

---

### Q: Why are my sub-theme's `.info` settings ignored? {#theme-settings-ignored}
**A: The database contains a copy of the theme settings and they take
precedence over theme settings stored in the `.info` file.**

This is actually quite a common issue whenever a theme's settings are saved
from the UI. This action stores the settings in the {variables} table of the
database. The workflow for theme_get_setting() merges the settings from the
database on top of the settings found in the theme's `.info` file.

You need to remove the variable `theme_[theme_name]_settings` from the database,
where `[theme_name]` is your sub-theme's machine name (e.g. if your sub-theme
machine name is `my_bootstrap_subtheme`, the variable name would be
`theme_my_bootstrap_subtheme_settings`).

The easiest way to do accomplish this task is to use the following [Drush]
command: `theme_my_bootstrap_subtheme_settings`

If you do not have or use [Drush], you will either have to manually delete this
variable from the database or create an [update hook](https://api.drupal.org/api/drupal/modules%21system%21system.api.php/function/hook_update_N/7)
in a custom module that deletes the variable for you:

```php
/**
* Remove the 'theme_my_bootstrap_subtheme_settings' variable.
*/
function hook_update_N() {
  variable_del('theme_my_bootstrap_subtheme_settings');
}
```

[Drush]: http://www.drush.org
[Drupal Bootstrap]: https://www.drupal.org/project/bootstrap
[Bootstrap Framework]: http://getbootstrap.com
[jQuery Update]: https://drupal.org/project/jquery_update
