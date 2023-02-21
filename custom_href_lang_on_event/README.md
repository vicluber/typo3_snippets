# Service implementation example

## Including helper

```
services:
  EffectiveWorld\Amixon\HrefLang\EventListener\OwnHrefLang:
    tags:
      - name: event.listener
        identifier: 'my-ext/ownHrefLang'
        after: 'typo3-seo/hreflangGenerator'
        event: TYPO3\CMS\Frontend\Event\ModifyHrefLangTagsEvent
```

## Including helper

```
<?php
namespace EffectiveWorld\Amixon\HrefLang\EventListener;

use TYPO3\CMS\Frontend\Event\ModifyHrefLangTagsEvent;

final class OwnHrefLang
{
   public function __invoke(ModifyHrefLangTagsEvent $event): void
   {
      $hrefLangs = $event->getHrefLangs();
      $request = $event->getRequest();

      // Do anything you want with $hrefLangs
      $hrefLangs = [
         'en-US' => 'https://example.org',
         'nl-NL' => 'https://example.org/nl'
      ];

      // Override all hrefLang tags
      $event->setHrefLangs($hrefLangs);

      // Or add a single hrefLang tag
      $event->addHrefLang('de-DE', 'https://example.org/de');
    }
}
```