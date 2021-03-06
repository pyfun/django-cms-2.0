Installation
============

Make sure that `cms`, `mptt` and `publisher` folders are on your pythonpath.

Apps
----

Add the following to your project's `INSTALLED_APPS` setting:

	INSTALLED_APPS = (
		...
		'cms',
		'cms.plugins.text',
		'cms.plugins.picture',
		'cms.plugins.link',
		'cms.plugins.file',
		'cms.plugins.snippet',
		'cms.plugins.googlemap',
		'mptt',
		...

		'publisher'
	)

Publisher needs to be at the end of the list.

Django-cms 2.0 is compatible with [django-reversion](http://code.google.com/p/django-reversion/) for versioning all the page content and its plugins. Put "reversion" in your INSTALLED\_APPS if you have it installed.

If you don't need all the plugins you can comment them out here.

Middleware
----------

Add the following middleware classes:

	MIDDLEWARE_CLASSES = (
    	...
    	'cms.middleware.page.CurrentPageMiddleware',
		'cms.middleware.user.CurrentUserMiddleware',
    	'cms.middleware.multilingual.MultilingualURLMiddleware',
    	...
    	)
    
If your site is not multilingual you can leave out `'cms.middleware.MutilingualURLMiddleware'`.

Template Context Processors
---------------------------

Add the following template context processors if not already present:

	TEMPLATE_CONTEXT_PROCESSORS = (
		"django.core.context_processors.auth",
		"django.core.context_processors.i18n",
		"django.core.context_processors.request",
		"django.core.context_processors.media",
		"cms.context_processors.media",
		...
	)

Templates
---------

If you don't have already a `gettext` stub in your `settings.py` file, insert the following line
near the beginning of the file but after any import statements that it may have:

	gettext = lambda s: s
	
This allows you to use `gettext` in `settings.py`.

Add some templates you want to use within Django-cms 2.0 that contain at least one
`{% placeholder %}` template tag to your `settings.py`:
	
	CMS_TEMPLATES = (
		('base.html', gettext('default')),
		('2col.html', gettext('2 Column')),
		('3col.html', gettext('3 Column')),
		('extra.html', gettext('Some extra fancy template')),
	)
	
For some template examples see the templates used in the example project.

Here's a quick example of what a template might look like:

	{% load cms_tags %}
	
	<div id="menu">{% show_menu 0 100 100 100 %}</div> 
	<div id="breadcrumb">{% show_breadcrumb %}</div>
	<div id="languagechooser">{% language_chooser %}</div>
	<div id="content">{% placeholder "content" %}</div>
	<div id="left_column">{% placeholder "left_column" %}</div>

Internationalization
--------------------

If your site is multilingual be sure to have the `LANGUAGES` setting present in your `settings.py` file:

	LANGUAGES = (
		('fr', gettext('French')),
		('de', gettext('German')),
		('en', gettext('English')),
	)

`urls.py`
-------


In your main `urls.py` add the following line at the **end** of the `urlpatterns` definition:

    urlpatterns = patterns('',
        url(r'^admin/', include(admin.site.urls)),
        ...
        ...
        url(r'^', include('cms.urls')),
    )

Other settings
--------------

For a list of all settings that can be overridden in your `settings.py` file have a look at `cms/settings.py`.

More information is available in the docs folder: [cms/docs/](http://github.com/digi604/django-cms-2.0/tree/master/cms/docs)
You may also want to have a look at the example project to see what settings it uses.

Media Files
-----------

**ATTENTION:**

If you don't use something like [django-appmedia](http://code.google.com/p/django-appmedia/)
be sure to copy the `cms/media/cms/` folder into your media directory or make a symbolic
link as appropriate.

South
-----

Django-cms 2.0 has support for [django-south](http://south.aeracode.org/). South is a database migration tool.
So if you get a new version of django-cms you also get the db-migrations automatically. Please read the documentation of south
first before you use it the first time. 

If there is a problem with python manage.py migrate:

Create a new ticket on github with your database engine (mysql), database type (MyIsam), south version

Quick fix:

Remove `south` from the installed apps. 
run `manage.py syncdb`
add `south` to the installed apps again.
run `manage.py migrate --fake`


Troubleshooting:
----------------

If you create a page and you don't see a page in the list view:

- Be sure you copied all the media files. Check with firebug and its "net" panel if you have any 404
