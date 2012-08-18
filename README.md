Django i18n helper
==================

Provides a simple way to visualize translated strings in Django templates 
by wrapping translated content with custom HTML and CSS. Therefore and
most important, helps you to visualize untraslated strings too.

This is particularly useful when internationalization is being added to a
project.


How does it works
-----------------

Django i18n helper is a common Django app that overriddes Django core functions
on load to provide the desired behavior.

The application will automatically detect when tests are being run and don't override any methods to preserve tests integrity.


Installation
------------

All you need to do is add "django-i18n-helper" to your installed apps and
activate the internationalization debug. In your settings.py make sure to have:

    INSTALLED_APPS = (
            ...,
            'django-i18n-helper'
            )

and

    I18N_HELPER_DEBUG  = True

django-i18n-helper provides a default behavior that consists in wrapping the
translated content with an HTML div with the following properties:

    <div class='i18n-helper' style='display: inline; background-color: #FAF9A7;'>Translated text</div>

This provides a soft highlight for transalted strings, but this behavior can be
modified within settings.py.


Customization
-------------

Some configuration variables are provided on order to customize how you want the translated strings to be wrapped.

#### I18N_HELPER_HTML

Defines a whole HTML block for wrapping the translations. This string will be
formatted (http://docs.python.org/library/stdtypes.html#str.format) with the
translated text. Thus every occurrence of "{0}" will be replaced with the
translation.

    I18N_HELPER_HTML = "<span class='highlight'>{0}</span>"

If I18N_HELPER_HTMLis not set, the code used will be

    <div class='i18n-helper' style='display: inline; background-color: #FAF9A7;'>{0}</div> 


#### I18N_HELPER_CLASS

Defines the class to use for the HTML div if I18N_HELPER_HTML is not used. Defaults to "i18n-helper".

    I18N_HELPER_CLASS = "my-custom-class"


#### I18N_HELPER_STYLE

Defines the inline CSS for the HTML div if no I18N_HELPER_HTML or
I18N_HELPER_CLASS have been set (case in which it's assumed that the styles
for the class provides the needed css). Defaults to ""display: inline; background-color: #FAF9A7;".

    I18N_HELPER_CLASS = "font-weight: bold; background-color: yellow;"


Screenshots
-----------

Graphical examples are sometimes the better way to understand how does something works or looks like. So here go two examples of how completely translated templates would look like, and two of how partially translated templates would.

Fully translated templates

<img src='https://dl-web.dropbox.com/get/Public/django-i18n-helper/Traslated1.png?w=c0f4b662'/>

<img src='https://dl-web.dropbox.com/get/Public/django-i18n-helper/Traslated2.png?w=09afdc01'/>

Partially translated templates. Note that it's also possible to see from the admin site which model fields haven't set the _verbose_name_ attribute to translate the field name.

<img src='https://dl-web.dropbox.com/get/Public/django-i18n-helper/Untraslated1.jpg?w=258a0054'/>

<img src='https://dl-web.dropbox.com/get/Public/django-i18n-helper/Untraslated2.png?w=0fbf93df'/>


Disclaimer Notes
----------------

The application should **only** be used when debugging code transalations, since it overrides the default Django HTML scaping mechanism and thus outputs unescaped (possibly undesired) code.
Besides, there are some warnings you should be aware of:

* You will see weird HTML within you buttons or inputs if you have things like &lt;input type="text" value="{% trans "Search" %}" ...&gt; Then the wrapping HTML of your translations will be shown _within_ the inputs or buttons. This will happen for sure in the admin site.

* Your database might not sync and show errors like "value too long for type character varying(50)". Set I18N_HELPER_DEBUG to False if this happens.

* Your migrations might not work and show errors like "value too long for type character varying(50)". Set I18N_HELPER_DEBUG to False if this happens.
