# edc-search
[![Build Status](https://app.travis-ci.com/samKenpachi011/edc-search.svg?branch=develop)](https://app.travis-ci.com/samKenpachi011/edc-search)
[![Coverage Status](https://coveralls.io/repos/github/samKenpachi011/edc-search/badge.svg?branch=develop)](https://coveralls.io/github/samKenpachi011/edc-search?branch=develop)


[![Language](https://img.shields.io/badge/Language-Python-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/Version-1.0.0-blue.svg)](https://github.com/samKenpachi011/edc-search/releases/tag/v1.0.0)
[![Log Scan Status](https://img.shields.io/badge/Log%20Scan-Passing-brightgreen.svg)](https://app.travis-ci.com/github/samKenpachi011/edc-search/logscans)

Add a slug field to models using the model mixin `SearchSlugModelMixin`. Specify the fields and/or properties to include in the `slug` in `search_slug_fields`:


    class TestModel(SearchSlugModelMixin, models.Model):

        search_slug_fields = ['f1', 'f2', 'f3']

        f1 = models.CharField(max_length=25, null=True)
        f2 = models.DateTimeField(null=True)
        f3 = models.IntegerField(null=True)
        f4 = models.CharField(max_length=25, null=True)

Fields in the `search_slug_fields` are converted to string in the slug:

    >>> obj = TestModel.objects.create(f1='run rabbit run!', f2=get_utcnow(), f3=12345)
    >>> obj.slug
    'run-rabbit-run!|2017-06-02 19:08:32.163520+00:00|12345'

Fields not listed are not included:

    >>> obj = TestModel.objects.create(f1='slug me', f4='don\'t slug me')
    >>> obj.slug
    'slug-me||'

`Null` fields are converted to `''`:

    >>> obj = TestModel.objects.create()
    >>> obj.slug
    '||'

You can use dotted syntax:

    class TestModel(SearchSlugModelMixin, models.Model):

        search_slug_fields = ['f1', 'name.first', 'name.last']

        f1 = models.CharField(max_length=25, null=True)

        def name(self):
            return FullName(first='Gore', last='vidal')

    >>> obj = TestModel.objects.create()
    >>> obj.slug
    '|Gore|Vidal'
