---
title: Tags
name: tags
date: 2018-01-21
tags: [ Tags, Listings, Pagination, Cloning, Virtual Pages, Templates ]
---
[% USE q = Qgoda %]
[% TAGS [@ @] %]
Tags are covered in great detail on the [Qgoda website](http://www.qgoda.net/en/docs/tags/), so you should look there for starters.

Even without reading the detailed documentation, the source code of [`_views/components/taglist.html`](https://github.com/gflohr/qgoda-essential/blob/master/_views/components/taglist..html) should be pretty self-explanatory.  In case you haven't guessed so already: It generates the list of tags in the right column of this page.

The tricky part is again the generation of an unknown, arbitrary number of tag pages.  And again, it's the [cloning feature](http://www.qgoda.net/en/docs/cloning/) that comes to our rescue.  This is the source code of `en/tags/index.md`:

<!--QGODA-NO-XGETTEXT-->
```yaml
---
virtual: 1
type: page
---
[% USE q = Qgoda %]

[% IF !asset.parent %]
    [% FOREACH tag IN q.taxonomyValues('tags') %]
      [% location = q.sprintf("/%s/tags/%s/index.html", 
                              asset.lingua,
                              tag.slugify(asset.lingua)) %]
      [% q.clone(location => location
                 plocation => location
                 title => '#' _ tag
                 tag => tag
                 start => 0) %]
    [% END %]
[% ELSE %]
  [% filters = { tags = ['icontains', tag] } %]
  [% INCLUDE components/listing.html filters = filters %]
[% END %]
```
<!--QGODA-NO-XGETTEXT-->

It bears an amazing similarity with the code for [archives]([@ q.llink(name = 'archives') @]).  The only noteworthy thing is the function `q.taxonomyValues()` which gives you the complete set of values for a certain taxonomy, in this case the taxonomy `tags`.

You can further restrict that by extracting only values from documents that have the same language and are of type posts (as an example):

```tt2
[% tags = q.taxonomyValues('tags' type = 'post' lingua = asset.lingua )]
```

Now move on to [@ q.lanchor(name = 'related') @] for learning how to list related documents.
