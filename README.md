# Cloudflare Cache #

This composer-installable example demonstrates how to purge Cloudflare cache when your live environment's cache is cleared.

## TODO
- Quicksilver script projects and the script name itself should be consistent in naming convention. - To rename after demonstrated working with existing filenames (underscores)

### Installation

While it's possible to use the script individually, this project is designed to be included from a site's `composer.json` file, and placed in its appropriate installation directory by [Composer Installers](https://github.com/composer/installers).

In order for this to work, you should have the following in your composer.json file:

```json
{
  "require": {
    "composer/installers": "^1"
  },
  "extra": {
    "installer-paths": {
      "web/private/scripts/quicksilver": ["type:quicksilver-script"]
    }
  }
}
```

The project can be included by using the command, where `{quicksilver-project}` represents the name of the Quicksilver script:

`composer require pantheon-quicksilver/{quicksilver-project}:^1`

If you are using one of the example PR workflow projects ([Drupal 8](https://www.github.com/pantheon-systems/example-drops-8-composer), [Drupal 9](https://www.github.com/pantheon-systems/drupal-project), [WordPress](https://www.github.com/pantheon-systems/example-wordpress-composer)) as a starting point for your site, these entries should already be present in your `composer.json`.


## Setup ##

- Copy `cloudflare-cache.json` to `files/private` of the *live* environment after updating it with your cloudflare info.
 - API key can be found in the `My Settings` page on the Cloudflare site.
 - I couldn't find zone id in the UI. I viewed page source on the overview page and found it printed in JavaScript.
- Add the example `cloudflare-cache.php` script to the `private/scripts` directory of your code repository.
- Add a Quicksilver operation to your `pantheon.yml` to fire the script after a deploy.
- Deploy through to the live environment and clear the cache!

Optionally, you may want to use the `terminus workflows watch` command to get immediate debugging feedback.


### Example `pantheon.yml`

Here's an example of what your `pantheon.yml` would look like if this were the only Quicksilver operation you wanted to use.

```yaml
api_version: 1

workflows:
  clear_cache:
    after:
      - type: webphp
        description: Cloudflare Cache
        script: private/scripts/cloudflare-cache.php
```

Note that you will almost always want to clear your CDN cache with the _after_ timing option. Otherwise you could end up with requests re-caching stale content. Caches should generally be cleared "bottom up".
