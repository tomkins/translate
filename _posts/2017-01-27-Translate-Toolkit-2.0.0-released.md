---
title: Translate Toolkit 2.0.0 released
category: releases
---

This release contains many improvements and bug fixes. While it contains many
general improvements, it also specifically contains needed changes and
optimizations for the upcoming [Pootle](http://pootle.translatehouse.org/)
2.8.0 and [Virtaal](http://virtaal.translatehouse.org) releases.

It is just over 4 weeks since the last release and there are many improvements
across the board. A number of people contributed to this release and we've
tried to credit them wherever possible (sorry if somehow we missed you).


Getting it and sharing it
=========================
- pip install translate-toolkit
- Please share this URL
  [http://toolkit.translatehouse.org/download.html](http://toolkit.translatehouse.org/download.html)
  if you'd like to tweet or post about the release.


2.0.0 vs 2.0.0rc3
=================

> **note**
>
> Given the large number of important changes, mostly Python 3 support,
> it was decided that the next version will be `2.0.0` instead of
> `1.14.0` as previously planned. So `1.14.0-rc1` could be considered to
> be `2.0.0b0`.


- `po2txt` works correctly again when `--threshold` option is passed (issue
  3499)
- Minor fixes for Mozilla .lang format
- Minor Python 3 fix


Major changes
=============

- Python 3 compatibility thanks to Claude Paroz
- Dropped support for Python 2.6
- Support for new l20n format
- Translate Toolkit can now easily be installed on Windows
- Changes in storage API to expose a more standardized API


Detailed changes
================

Python 3 support
----------------

- Translate Toolkit went through a massive code cleanup looking forward Python
  3 compatibility. There might be quirks that need to be fixed, so please test
  and [report any issue](https://github.com/translate/translate/issues/new)
  you might find.
- Python 3.3-3.5 is now supported.


Requirements
------------

- `lxml` requirement was raised to 3.5.0 in order to simplify code.
- Updated and pinned requirements
- Removed misleading extra requirements files


Formats and Converters
----------------------

- PO

  - msgid comments (KDE style) are only parsed from msgid now.
  - Fixed parsing of PO files with first entry in unicode
  - Fixed parsing of locations with percent char

- XLIFF

  - Unaccepted ASCII control characters are now escaped in XLIFF

- DTD
  - Newlines are now skipped when parsing (issue 3390).
  - Invalid ampersands are not scrubbed anymore.
  - label+accesskey is now only extracted if it is not followed by space.

- Properties

  - Keys can contain delimiters if they are properly wrapped (issue 3275).
  - Fix control characters escaping for utf-8 encoding.
  - `po2prop` removes fully untranslated units if required
  - `po2prop` skips first entry in PO file (issue 3463)

- Mozilla .lang

  - `{ok}` marker is now more cleanly removed
  - Always output last unit followed by trailing newlines
  - Added support for headers and tag comments
  - `MAX_LENGTH` is now parsed into comment
  - File line endings are now remembered, defaulting to Unix LF

- Mozilla's l20n

  - Added this new format storage class
  - Added variants and traits support
  - Added new converters `l20n2po` and `po2l20n`

- Android

  - Unknown locales no longer produce failures.
  - Simplify newlines handling as the format now handles \n and newline equally
    (issue 3262)
  - Moved all namespaces to <resources> element.
  - Simplified newlines handling

- ODF

  - `odf2xliff` now extracts all the text (issue 3239).

- ts

  - XML declaration is written with double quotes.
  - Self-closing for 'location' elements are not output anymore.

- JSON

  - Output now includes a trailing newline.
  - Unit ordering is maintained (issue 3394).
  - Added `--removeuntranslated` option to `json2po`

- YAML

  - YAML format support has been added.

- txt

  - `po2txt` works correctly again when `--threshold` option is passed (issue
    3499)

- ical

  - Enabled this format for Python 3 too.

- TermBase eXchange (TBX)
  - `tbx2po` converter added
  - Added basic support for Parts of Speech and term definitions.

-   Fixed error when writing back to the same file (issue 3419).


Filters and Checks
------------------

- Added the ability to skip some checks for some languages in specific checkers
- ``accelerators`` check reports an error if accelerator is present for several
  Indic languages in ``MozillaChecker`` checker.
- Added `l20nChecker` to do custom checking for Mozilla's new l20n format.
- LibreOffice checker no longer checks for Python brace format (issue 3303).
- LibreOffice validxml check correctly matches self-closing tags.
- Numbers check now handles non latin numbers. Support for non latin numbers
  has been added for Arabic, Assamese, Bengali and Persian languages.
- Fixed issue that prevented standard checks from being used in Pootle with
  default settings.
- Fixed missing attribute warning displayed when using `GnomeChecker`,
  `LibreOfficeChecker` and `MozillaChecker` checkers.
- Added language specific ``RomanianChecker``.


Tools
-----

- `posegment` now correctly segments Japanese strings with half width
  punctuation sign (issue 3280).
- `pocount` now outputs csv header in one line. It also outputs using color.
- `buildxpi` was adjusted to current Mozilla needs


Languages
---------

- Fixed plural form for Montenegro, Macedonian, Songhay, Tajik, Slovenian and
  Turkish.
- Added plural forms for Bengali (Bangladesh), Konkani, Kashmiri, Sanskrit,
  Silesian and Yue (Cantonese).
- Added valid accelerators for Polish.
- Renamed Oriya to Odia.
- Altered Manipuri name to include its most common name Meithei.
- Added language settings for Brazilian Portuguese.
- Added Danish valid accelerators characters (issue 3487).
- Added additional special characters for Scottish Gaelic.


Setup
-----

- Fixed Inno Setup builds allowing to easily install Translate Toolkit on
  Windows using the `pip` installer. Commands are compiled to .exe files.


API changes
-----------

- Dropped `translate.misc.dictutils.ordereddict` in favor of
  `collections.OrderedDict`.
- Added encoding handling in base `TranslationStore` class exposing a single
  API.
- Encoding detection in `TranslationStore` has been improved.
- Standardized UnitClass definition across `TranslationStore` subclasses.
- `translate.misc.multistring.multistring`:

  - Fixed list coercion to text
  - Fixed comparison regression with multistrings (issue 3404).
  - Re-added `str` method (issue 3428).
  - Fixed `__hash__` (issue 3434).


API deprecation
---------------

- Passing non-ASCII bytes to the `multistring` class has been deprecated, as
  well as the `encoding` argument to it.
  Applications should always construct `multistring` objects by passing
  characters (`unicode` in Python 2, `str` in Python 3), not bytes. Support
  for passing non-ASCII bytes will be removed in the next version.
- `TxtFile.getoutput()` and `dtdfile.getoutput()` have been deprecated.
  Either call `bytes(<file_instance>)` or use the
  `file_instance.serialize()` API if you need to get the serialized store
  content of a `TxtFile` or `dtdfile` instance.


General
-------

- Dropped support for Python 2.6 since it is no longer supported by the Python
  Foundation. Sticking to it was making us difficult to maintain code while we
  move to Python 3.
- Misc docs cleanups.
- Added more tests.
- Increased Python code health.
- Legacy, deprecated and unused code cleansing:

  - Dropped code for no longer supported Python versions.
  - Removed unused code from various places across codebase.
  - The legacy `translate.search.indexing.PyLuceneIndexer1` was removed.
  - The deprecated `translate.storage.properties.find_delimiter()` was
    removed and replaced by the
    `translate.storage.properties.Dialect.find_delimiter()` class method.
  - Python scripts are now available via `console_scripts` entry point, thus
    allowing to drop dummy files for exposing the scripts.


...and loads of general code cleanups and of course many many bugfixes.


Contributors
============

This release was made possible by the following people:

Claude Paroz, Leandro Regueiro, Dwayne Bailey, Michal Čihař, Taras Semenenko,
Ryan Northey, Stuart Prescott, Kai Pastor, Julen Ruiz Aizpuru, Friedel Wolff,
Hiroshi Miura, Thorbjørn Lindeijer, Melvi Ts, Jobava, Jerome Leclanche, Jakub
Wilk, Adhika Setya Pramudita, Zibi Braniecki, Zdenek Juran, Yann Diorcet, Nick
Shaforostoff, Jaka Kranjc, Christian Lohmaier, beernarrd.

And to all our bug finders and testers, a Very BIG Thank You.