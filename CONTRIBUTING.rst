============
CONTRIBUTING
============

Contributions are welcome and greatly appreciated!

Ways to Contribute:

- `Adding Data`_
- `Cleaning Data`_
- `Reporting bugs`_
- `Writing Documentation`_


Adding Data
-----------

JSON Objects
~~~~~~~~~~~~

Each video contributed to `pyvideo/data`_ must have its own JSON object
containing details about the video.
For example, here's a JSON object containing data about a talk at PyGotham 2015::

    {
        "copyright_text": "CC BY-SA",
        "description": "Computational tools are becoming increasingly ...",
        "duration": 3456,
        "language": "eng",
        "recorded": "2015-08-10",
        "related_urls": [
            {
              "label": "talk slides",
              "url": "http://example.com/the/slide/deck.pdf"
            },
            {
              "label": "example notebook",
              "url": "http://example.com/a/notebook.nb"
            }
        ],
        "speakers": [
            "Eric Schles"
        ],
        "tags": ["social good", "tools", "computational"],
        "thumbnail_url": "https://i.ytimg.com/vi/APC5HvHZaf0/hqdefault.jpg",
        "title": "Building tools for Social good",
        "videos": [
            {
                "type": "youtube",
                "url": "http://youtu.be/APC5HvHZaf0"
            },
            {
                "size": 123456789,
                "type": "mp4",
                "url": "http://example.com/some/place/on/the/web.mp4"
            }
        ]
        ...
    }

The JSON object for each video **must** define values for these keys ...

==================================     ==================================     ==================================
key                                    value type                             details
==================================     ==================================     ==================================
description                            rST string                             (see below)
----------------------------------     ----------------------------------     ----------------------------------
speakers                               array of strings
----------------------------------     ----------------------------------     ----------------------------------
thumbnail_url                          string
----------------------------------     ----------------------------------     ----------------------------------
title                                  string
----------------------------------     ----------------------------------     ----------------------------------
recorded                               string                                 (YYYY-MM-DD)
----------------------------------     ----------------------------------     ----------------------------------
videos                                 array of objects
==================================     ==================================     ==================================

... and can optionally define values for a number of other keys. For example, ...

==================================     ==================================     ==================================
key                                    value type                             details
==================================     ==================================     ==================================
copyright_text                         string
----------------------------------     ----------------------------------     ----------------------------------
duration                               integer
----------------------------------     ----------------------------------     ----------------------------------
language                               string                                 (`ISO 639-3`_ coded)
----------------------------------     ----------------------------------     ----------------------------------
quality_notes                          string
----------------------------------     ----------------------------------     ----------------------------------
related_urls                           array of strings
----------------------------------     ----------------------------------     ----------------------------------
tags                                   array of strings                       (see below)
==================================     ==================================     ==================================

For a full schema of a video JSON object, please see
https://github.com/pyvideo/data/blob/master/.schemas/video.json

.. _ISO 639-3: https://en.wikipedia.org/wiki/ISO_639-3

*Requirements of the Description String*

The description string for a video must be a valid reStructuredText (rST)
document. That is, the string keyed by "description" in a JSON object must be
parsible by a reStructuredText (rST) parser. `pyvideo/data`_ uses docutils'
``docutils.parsers.rst.Parser`` to test the validity of the description and
summary strings for each video.

If any errors, syntax or otherwise, occur while parsing the contents of
description string, the pull request will fail its validation tests
(which are run automatically upon pull request creation). All tests must
pass for a pull request to be merged in.

The good news is that, most descriptions with evenly indented paragraphs and
lists are already valid rST! If you need to brush up on your rST, check out
these resources:

- A ReStructuredText Primer (http://docutils.sourceforge.net/docs/user/rst/quickstart.html)
- Quick reStructuredText (http://docutils.sourceforge.net/docs/user/rst/quickref.html)
- Sphinx's reStructuredText Primer (http://www.sphinx-doc.org/en/stable/rest.html)
- reStructuredText Markup Specification (http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html)

*Title*

Since the list of speakers, event name, and other metadata are captured elsewhere in each video's JSON object,
it is suggested that the value of the ``title`` string contain only the title of the video and not contain any
other information about the video.

*Speakers*

If a speaker has multiple names - for instace, their speaker profile includes both the Chinese character representation
and the Latin alphabet representation of their name - each variation on their name should be recorded as a separate list
item. This enables users to search by all variations of the speaker's name in our web interface.

*Tags*

For consistency, tag strings should be:

-  lowercase
-  space separated

Even in the case of proper nouns or brand names, tag strings should be lowercase. Also, if adding a tag, `use existing tags <http://pyvideo.org/tags.html>`_ where possible, to improve consistency. If an existing tag does not comply with the standards listed above, please create a new tag that does. Correcting the non-compliant tag would also be greatly appreciated, but that can be a separate step/commit.

Examples:

-  `docker`
-  `lightning talk`
-  `continuous integration`


*Related URLs*

If there are other resources available and related to the video (slide decks, etc),
it is suggested that they are referenced in the ``related_urls`` array of URLs
rather than in the description.

Data Completeness
~~~~~~~~~~~~~~~~~

If an issue is tagged as `minimal download` in the Issue Tracker, some data has been loaded, but is incomplete.
To address the ticket check the following requirements to consider talk data reasonably complete

* `Title`: Only contains the actual talk title (No speaker name, no year, no conference name)
* `Description`: Correct restructured text, only describing the talk content (no speaker name/bio, no conference name/data)
* `Tags`: Definitely tag a video if it is either a `keynote` or a collection of `lightning talks`
* `Language`
* `Speakers`

Even better:

* Add meaningful `tags` related to talk content
* `Related urls`: Add talk slides or talk repository links where available

Above and beyond:

* `Recorded`: Check to make sure the date is correct (rather than just the estimation from minimal download) or add datetime with time zone
* `Related urls`: Add any url mentioned in the talk


JSON Files
~~~~~~~~~~

Each video's JSON object must live in its own file
(eg. ``building-tools-for-social-good.json``). For example, in the case of
``building-tools-for-social-good.json``, the JSON file would contain the single
JSON object listed above. A JSON file can be named whatever you like so long
as it ends in ``.json`` and follows the same slugification rules that apply to
categories. A semantic file name is encouraged.

Pretty print is important! In order to maintain this data with ease,
it needs to be easily parseable by the human eye. Thus, all contributions are
requested to be in pretty-printed format. Thankfully, Python makes this an easy task.
To convert a single file of ugly JSON to a file of pretty JSON, you can use the
following command from the root of your local clone of the `pyvideo/data`_ repo::

    $ python tools/reserialize.py path/to/file.json

If you added a lot of data and don't want to run the above command for each file,
you can use the following command to re-serialize the whole repo::

    $ python tools/reserialize.py --all .

.. note:: Before using the tools, you should install some packages. In order to
   obtain them, you can run the following command::
       $ pip install -r tools/requirements.txt

Finally, video JSON files should go in a directory called ``videos`` that is
itself inside a category directory. For example::

    root
    |_ pygotham-2015/
       |_ category.json
       |_ videos/
          |_ building-tools-for-social-good.json
          |_ all-speed-no-greed.json
          .
          .
          .

Categories
~~~~~~~~~~

All video JSON files must be placed in a category specific sub-directory.
For example, JSON files for PyGotham 2015 would go in ``pygotham-2015/videos/``.

Categories are most commonly synonymous with the event at which the video was
recorded. However, a category can be any ASCII string containing only
alphanumeric characters and the dash character (ie. ``-``).  For example, a
category could be a user group's name, a podcast, or the SHA256 hash of your
genetic material; up to you. Please note that a semantic category name is encouraged.

Inside of each category directory is a file called ``category.json``. This file
contains a single JSON object that stores metadata regarding the category.

The JSON object for each category **must** define only one key ...

==================================     ==================================
key                                    value type
==================================     ==================================
title                                  string
==================================     ==================================

For a full schema of a category JSON object, please see https://github.com/pyvideo/data/blob/master/.schemas/category.json

Creating Data From Youtube Channel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can easily create some basic data from a youtube list

Install requirements::

   $ pip install -r tools/requirements.txt

Get youtube api key from google console, please see https://developers.google.com/youtube/registering_an_application
Set GOOGLE_API_KEY environment::

   $ export GOOGLE_API_KEY=what_you_get_from_google_console

Run ./tools/youtube.py -l playlist ID # ( in this case pycon jp 2015 )::

   $ python tools/youtube.py -l PLMkWB0UjwFGkE3BIm4qpwaK5oPrmLLJoT

Find created directory, rename it and fill in missing data.


Pull Request
~~~~~~~~~~~~

Once your video JSON files and category JSON file are ready to be added to
pyvideo's collection, take the following steps:

#. Fork this repo if you haven't already.
#. Clone from your forked repo.
#. Add your category directory (containing the JSON files) into the root of the repo.
#. Commit your changes and push them up to your fork.
#. Run your tests locally by activating your venv and running ``make test``
#. Address any test failures. Common test failures from the youtube import process come from unescaped special characters and odd description formatting.
#. Issue a Pull Request of your changes to this repo.

And you're done! So long as you've followed this guide, your Pull Request (PR)
should be ready for review and merger. Your changes will be visible on
pyvideo.org within a few days after the PR is merged.

Cleaning Data
-------------

See a bug, typo, or problem with the data and have a minute to fix it? Great!
Please fork this repo, make the change, and submit a pull request.

Reporting Bugs
--------------

Report bugs at:

https://github.com/pyvideo/data/issues

If you are reporting a bug about incorrect data, please include:

* The directory, file or files that are relevant.
* The data that is incorrect.
* Values for the corrected data if you can provide them.

If you are reporting a bug about things to add, please include:

* The name of the thing to add (the conference name, the user group name, etc).
* A description of the thing.
* Any urls where we can find additional details about the thing.
* The url for the video material.

.. Note::

   Please remember that this is a volunteer-driven project!

   All work is done on a volunteer basis, so if you write up an issue, it may
   sit there for a while.

   If you see an issue you can help with, please pitch in! If you don't, don't
   expect anyone else to, either.


Writing Documentation
---------------------

Our documentation can always be better. What questions did you have that you
think other people might have that aren't answered in the documentation? Were
you able to find what you were looking for? Was documentation in weird
unexpected places? Are there typos? Are examples helpful? Are examples missing?

We could always use more documentation whether that's part of the official docs,
comments and docstrings in the code or even elsewhere on the web in blog posts,
articles, tweets and other things like that.

**Thanks so much for contributing to your worldwide Python community!**

..  _`pyvideo/data`: https://github.com/pyvideo/data
