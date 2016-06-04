# Summary Cards

[![Build Status](https://secure.travis-ci.org/SemanticMediaWiki/SummaryCards.svg?branch=master)](http://travis-ci.org/SemanticMediaWiki/SummaryCards)
[![Code Coverage](https://scrutinizer-ci.com/g/SemanticMediaWiki/SummaryCards/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/SemanticMediaWiki/SummaryCards/?branch=master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/SemanticMediaWiki/SummaryCards/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/SemanticMediaWiki/SummaryCards/?branch=master)
[![Latest Stable Version](https://poser.pugx.org/mediawiki/summary-cards/version.png)](https://packagist.org/packages/mediawiki/summary-cards)
[![Packagist download count](https://poser.pugx.org/mediawiki/summary-cards/d/total.png)](https://packagist.org/packages/mediawiki/summary-cards)
[![Dependency Status](https://www.versioneye.com/php/mediawiki:summary-cards/badge.png)](https://www.versioneye.com/php/mediawiki:summary-cards)

Summary Cards (a.k.a SUC) is a simple extension for displaying content summaries on
hovered links.

The content of a Summary Card on a hovered link is created by a template that is
assigned to the namespace the link belongs and requested via [Ajax][ajax] to improve
display responsiveness.

The extension does not require [Semantic MediaWiki][smw] but it is highly recommended to
use them together in order for summaries to generate individual content (e.g. property
that contains short description, image property, known keywords, modification date etc.)
while building a summary.

## Requirements

- PHP 5.5 or later
- MediaWiki 1.24 or later

## Installation

The recommended way to install SummaryCards is by using [Composer][composer] with
an entry in MediaWiki's `composer.json`.

```json
{
	"require": {
		"mediawiki/summary-cards": "~1.0"
	}
}
```
1. From your MediaWiki installation directory, execute
   `composer require mediawiki/summary-cards:~1.0`
2. Navigate to _Special:Version_ on your wiki and verify that the package
   have been successfully installed.

## Usage

![image](https://cloud.githubusercontent.com/assets/1245473/15775040/0033ad4c-2980-11e6-9514-007afc0ed630.png)

Logged-in users can individually decide whether or not to display a summary card while
the setting `$GLOBALS['sucgEnabledForAnonUser']` is provided to disable Summary Cards
for anon users globally.

The setting for a user can be found under `Preference -> Appearance`.

### Templates and content summaries

Summaries are enabled on a per namespace basis by assigning a template that specifies
the expected content.

```
$GLOBALS['sucgEnabledNamespaceWithTemplate'] = array(
	NS_MAIN         => 'Hovercard',
	NS_HELP         => 'Hovercard',
	NS_FILE         => 'Hovercard-File',
	NS_CATEGORY     => 'Hovercard-Category',
	SMW_NS_PROPERTY => 'Hovercard-Property',
);
```

If the setting includes reference to `SMW_NS_PROPERTY` then it should be added after
the `enableSemantics` in order for namespaces to be registered appropriately.

The [template][temp] document contains some simple examples on how to create dynamic
content summaries.

### Links

Summary Cards uses some ruleset to match only legitimate links (external as well as
interwiki links are generally disabled) for preparing a summary display, yet it
can happen that the set has missed a certain link type and if possible needs adjustment.

Summary cards on the `SMW_NS_PROPERTY` namespace are only displayed when no
other highlighter (e.g. from SMW core) is available to avoid competing tooltips.

### Cache

To avoid unnecessary API requests, a client can use the local cache with
`$GLOBALS['sucgTooltipRequestCacheLifetime']` defining the during of how long
data are to be kept before a new request is made (is set to `30 min` by
default, `false` to disable it).

While client-side browser caching may help to avoid repeated requests, a
similar request from a different browser would still cause a content parse.
To leverage on existing summaries that have been created by other users,
`$GLOBALS['sucgBackendParserCacheType']` can be set to create cacheable
server-side content without deploying a challenging infrastructure (using
squid or varnish).

If a subject (which equals the hovered link) and/or its assigned template are
altered then related cache items are evicted in order for content to be
updated (content that is locally cached are exempted from the update).

## Contribution and support

If you want to contribute work to the project please subscribe to the developers mailing list and
have a look at the contribution guideline.

* [File an issue](https://github.com/SemanticMediaWiki/SummaryCards/issues)
* [Submit a pull request](https://github.com/SemanticMediaWiki/SummaryCards/pulls)
* Ask a question on [the mailing list](https://semantic-mediawiki.org/wiki/Mailing_list)
* Ask a question on the #semantic-mediawiki IRC channel on Freenode.

## Tests

This extension provides unit and integration tests that are run by a [continues integration platform][travis]
but can also be executed using `composer phpunit` from the extension base directory.

## License

[GNU General Public License, version 2 or later][gpl-licence].

[gpl-licence]: https://www.gnu.org/copyleft/gpl.html
[travis]: https://travis-ci.org/SemanticMediaWiki/SummaryCards
[smw]: https://github.com/SemanticMediaWiki/SemanticMediaWiki
[composer]: https://getcomposer.org/
[ajax]: https://en.wikipedia.org/wiki/Ajax_(programming)
[temp]: https://github.com/SemanticMediaWiki/SummaryCards/blob/master/docs/templates.md
