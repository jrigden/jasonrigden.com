---
title: "Parsing RSS feeds with Python"
date: 2016-01-23T11:38:35-07:00
draft: false
categories : [ "Tutorial" ]
tags: [ "RSS", "Python"]

---


There are several option to parse RSS feeds for python. I wrote one of those options.

[**pyPodcastParser**](https://github.com/jrigden/pyPodcastParser) is a podcast parser. It should parse any XML RSS file, but it specializes in parsing podcast feeds. It does not parse ATOM feeds.

### **Featuring**

*   [Automated testing with Travis CI](https://travis-ci.org/jrigden/pyPodcastParser)
*   [100% test coverage as measured by Coveralls](https://coveralls.io/github/jrigden/pyPodcastParser?branch=master)
*   [An MIT license](https://opensource.org/licenses/MIT)
*   Compatibility with python 2 and python 3
*   [Available on pyi](https://pypi.python.org/pypi/pyPodcastParser)
*   Support of RSS 2.0
*   Support of iTunes podcast tags
*   OS independent

### Installation

<pre name="560a" id="560a" class="graf graf--pre graf-after--h3">pip install pyPodcastParser</pre>

### Usage

<pre name="89af" id="89af" class="graf graf--pre graf-after--h3">from pyPodcastParser.Podcast import Podcast  
import requests  

response = requests.get('https://some_rss_feed')  
podcast = Podcast(response.content)</pre>

### Objects and their Useful Attributes

**_Note:_**

All attributes with empty or nonexistent element will have a value of None._All attributes with empty or nonexistent element will have a value of None._

*   _Attributes are generally strings or lists of strings, because we want to record the literal value of elements._
*   _The cloud element aka RSS Cloud is not supported as it has been superseded by the superior PubSubHubbub protocol_

#### Podcast

*   categories (list) A list for strings representing the feed categories
*   copyright (string): The feed’s copyright
*   creative_commons (string): The feed’s creative commons license
*   items (list): A list of Item objects
*   description (string): The feed’s description
*   generator (string): The feed’s generator
*   image_title (string): Feed image title
*   image_url (string): Feed image url
*   image_link (string): Feed image link to homepage
*   image_width (string): Feed image width
*   image_height (Sample H4string): Feed image height
*   itunes_author_name (string): The podcast’s author name for iTunes
*   itunes_block (boolean): Does the podcast block itunes
*   itunes_categories (list): List of strings of itunes categories
*   itunes_complete (string): Is this podcast done and complete
*   itunes_explicit (string): Is this item explicit. Should only be “yes” and “clean.”
*   itune_image (string): URL to itunes image
*   itunes_keywords (list): List of strings of itunes keywords
*   itunes_new_feed_url (string): The new url of this podcast
*   language (string): Language of feed
*   last_build_date (string): Last build date of this feed
*   link (string): URL to homepage
*   managing_editor (string): managing editor of feed
*   published_date (string): Date feed was published
*   pubsubhubbub (string): The URL of the pubsubhubbub service for this feed
*   owner_name (string): Name of feed owner
*   owner_email (string): Email of feed owner
*   subtitle (string): The feed subtitle
*   title (string): The feed title
*   ttl (string): The time to live or number of minutes to cache feed
*   web_master (string): The feed’s webmaster

#### Item

*   author (string): The author of the item
*   comments (string): URL of comments
*   creative_commons (string): creative commons license for this item
*   description (string): Description of the item.
*   enclosure_url (string): URL of enclosure
*   enclosure_type (string): File MIME type
*   enclosure_length (integer): File size in bytes
*   guid (string): globally unique identifier
*   itunes_author_name (string): Author name given to iTunes
*   itunes_block (boolean): It this Item blocked from itunes
*   itunes_closed_captioned: (string): It is this item have closed captions
*   itunes_duration (string): Duration of enclosure
*   itunes_explicit (string): Is this item explicit. Should only be “yes” and “clean.”
*   itune_image (string): URL of item cover art
*   itunes_order (string): Override published_date order
*   itunes_subtitle (string): The item subtitle
*   itunes_summary (string): The summary of the item
*   link (string): The URL of item.
*   published_date (string): Date item was published
*   title (string): The title of item.

### Bugs & Feature Requests

I would like your help to break pyPodcastParser. P[lease send me any bug reports](https://github.com/jrigden/pyPodcastParser/issues/new). I would also like to hear of your [feature requests](https://github.com/jrigden/pyPodcastParser/issues/new).
