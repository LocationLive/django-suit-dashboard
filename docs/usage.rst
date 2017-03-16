=====
Usage
=====

Some real-time widgets and box examples.

.. code:: python

    import random
    from suit_dashboard.admin import realtime


    class CustomRealtimeWidget(Widget):
        html_id = 'custom_widget'
        name = 'Custom Widget'
        template = 'app/dashboard/display/highcharts_custom.html'
        url_name = 'custom_rtw'
        url_regex = 'realtime/custom_rtw'
        max_points = 30
        time_interval = 1000

        # initial content
        def get_content(self):
            highcharts_dict = {
                'chart': {
                    'type': 'spline',
                    'marginRight': 10,
                },
                'title': {
                    'text': 'Live random data'
                },
                'xAxis': {
                    'type': 'datetime',
                    'tickPixelInterval': 150
                },
                'yAxis': {
                    'title': {
                        'text': 'Value'
                    },
                    'plotLines': [{
                        'value': 0,
                        'width': 1,
                        'color': '#808080'
                    }]
                },
                'series': [{
                    'name': 'Random data',
                    'data': []
                }]
            }
            return highcharts_dict

        # get random points
        def get_updated_content(self):
            return (
                (timezone.make_aware(
                    datetime.now(), timezone.get_current_timezone()
                ) - timezone.make_aware(
                    datetime(1970, 1, 1),
                    timezone.get_current_timezone()
                )).total_seconds() * 1000.0,
                random.choice(range(0, 25)),
            )


    def lorem_ipsum_generator():
        for word in "Lorem ipsum dolor sit amet, consectetur adipiscing elit. In odio ligula, consequat id libero in, efficitur suscipit dolor. Vivamus interdum justo tincidunt libero pretium congue. Sed luctus augue vitae arcu imperdiet, vitae pellentesque elit auctor. In hac habitasse platea dictumst. Nunc fermentum sem vel neque maximus viverra. Praesent tortor leo, ultricies ut bibendum in, luctus nec felis. Donec id eleifend eros. Phasellus congue sollicitudin erat sed dapibus.".split(' '):
            yield word
        yield '<br>'
        yield '<br>'
        for word in "Nullam ullamcorper dictum velit, et consequat nunc aliquam sed. Aenean ut nibh nunc. In facilisis sit amet lectus non tempor. Integer non lacinia lacus, vel venenatis dui. Sed a luctus neque. Maecenas interdum, tellus sed gravida semper, metus massa tempus quam, non tristique dui arcu nec leo. Donec iaculis ut mauris eu venenatis. Donec eu pulvinar purus, nec viverra quam. Vivamus ultricies pretium hendrerit. Fusce lobortis et mauris ut consequat. Nullam eu aliquet neque. Aenean mauris quam, cursus eget tellus sed, fringilla eleifend orci. Curabitur a pretium enim.".split(' '):
            yield word
        yield '<br>'
        yield '<br>'
        for word in "Vivamus pulvinar vehicula sem, sed cursus lorem volutpat sit amet. In id lacinia quam. Aliquam ligula velit, scelerisque in finibus id, efficitur sit amet quam. Nam sagittis semper nisl sed pharetra. Praesent ex lorem, vestibulum ac ipsum quis, finibus molestie lorem. Vivamus vehicula lacus in leo fringilla, nec fermentum nibh fermentum. Mauris tincidunt, metus et venenatis auctor, sem felis sagittis odio, a volutpat erat eros luctus enim. Nunc in velit a arcu rhoncus ultricies. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Phasellus sit amet auctor libero. Donec vitae est sodales, tincidunt magna in, ultrices arcu. Praesent blandit enim a nibh tristique placerat. Aliquam cursus euismod eros, vel ultrices massa ultrices id. Mauris eu malesuada mi. Integer ut diam semper, gravida odio ac, bibendum velit. Curabitur eget ante at eros iaculis aliquam nec commodo nisi.".split(' '):
            yield word
        yield '<br>'
        yield '<br>'
        for word in "Praesent pulvinar efficitur lorem quis feugiat. Nam pharetra iaculis lacus, ac suscipit dui. Sed aliquam feugiat purus, at interdum velit faucibus sed. Suspendisse bibendum est erat, sed pulvinar nulla laoreet sed. Aliquam nec lacus sed ante cursus consectetur at id dui. Suspendisse potenti. In posuere arcu vel erat sollicitudin, nec auctor eros vehicula. Aliquam erat volutpat. Proin ultrices blandit ex at tincidunt. Quisque porta nec est sit amet facilisis. Proin euismod nisi mattis dui dictum dictum. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nunc vestibulum suscipit lorem, non tristique quam bibendum et. Fusce hendrerit sem a rhoncus congue.".split(' '):
            yield word
        yield '<br>'
        yield '<br>'
        for word in "Praesent maximus, massa ut porttitor egestas, enim quam tincidunt lorem, in volutpat erat metus ut quam. Nulla sodales accumsan nisl. Pellentesque sollicitudin lectus libero, et auctor diam mattis molestie. Maecenas sed venenatis dolor. Interdum et malesuada fames ac ante ipsum primis in faucibus. Aliquam id blandit nisl, ac fermentum arcu. Aliquam magna lorem, commodo id velit ornare, tincidunt ultricies turpis. Vivamus non velit nec erat accumsan venenatis. Phasellus molestie massa suscipit felis feugiat vestibulum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Curabitur pellentesque mattis euismod. Donec faucibus id nulla nec imperdiet. Quisque interdum justo quis nisl feugiat, sed vehicula eros mollis. Nullam lorem justo, maximus nec viverra vitae, consequat id lectus.".split(' '):
            yield word


    class ProgressiveLoremIpsumWidget(RealTimeWidget):
        html_id = 'lorem_id'
        name = 'Quick Lorem'
        url_name = 'quick_lorem'
        url_regex = 'realtime/quick_lorem'
        time_interval = 100
        template = 'app/dashboard/display/progressive_paragraph.html'

        def __init__(self, *args, **kwargs):
            super().__init__(*args, **kwargs)
            self.generator = None
            self.reset_generator()

        def reset_generator(self):
            self.generator = lorem_ipsum_generator()

        def get_content(self):
            return ''

        def get_updated_content(self):
            try:
                return next(self.generator)
            except StopIteration:
                return 'STOP'


    class CustomBox(Box):
        lorem =
        widgets = [
            realtime(CustomRealtimeWidget()),
            realtime(ProgressiveLoremIpsumWidget())
        ]

        def get_widgets(self):
            self.widgets[1].reset_generator()
            return self.widgets


First, mandatory steps
======================

To add custom views in your admin interface, you will need 4 things:

- an AdminSite instance
- loading it in your main URLs module
- change ``django.contrib.admin`` to
  ``django.contrib.admin.apps.SimpleAdminConfig`` in ``INSTALLED_APPS``
- the views (class-based ones)

Here is an example of AdminSite:

.. code:: python

    from django.contrib.admin.sites import AdminSite
    from django.conf.urls import url

    from .views import CustomView


    class DashboardSite(AdminSite):
        def get_urls(self):
            urls = super(DashboardSite , self).get_urls()
            custom_urls = [
                url(r'^$', self.admin_view(CustomView.as_view()), name='index'),
            ]

            del urls[0]
            return custom_urls + urls

As you can see, the first URL (leading to the main page of the interface)
has been replaced by a link to our custom view. It will allow us to add more
links to more content.

.. note::

    If you want to keep the original main page, just comment ``del urls[0]``
    and change the regular experssion of the custom URL (something like
    ``r'^dashboard/'``).
    Note that in this case, you will have to add a link to the dashboard
    somehow in the basic main page, or you will need to enter the URL manually
    in your browser.

Here the only added view is imported from a ``views`` module. We will see later
how it is written.

Before loading your admin site, change ``django.contrib.admin`` to
``django.contrib.admin.apps.SimpleAdminConfig`` in ``INSTALLED_APPS``.
Since Django Suit Dashboard works well with Django Suit, you can write
something like the following to test with and without Suit rapidly.

.. code:: python

    SUIT = True
    # SUIT = False

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'proyekt',
        'suit_dashboard'
    ]

    if SUIT:  # add suit and replace admin with SimpleAdminConfig
        INSTALLED_APPS = [
            'suit',
            'django.contrib.admin.apps.SimpleAdminConfig'
        ] + INSTALLED_APPS[1:]

Just switch on and off ``SUIT`` boolean to test it out.

Once your admin site is set up and your installed apps ok,
load your admin site in your main URLs module:

.. code:: python

    from django.contrib import admin

    from .site import DashboardSite

    admin.site = DashboardSite()
    admin.sites.site = admin.site
    admin.autodiscover()


Views
=====

Django Suit Dashboard provides a view called ``DashboardView``, importable
from ``suit_dashboard.views``. You can inherit of this base view to create
your own views.

.. code:: python

    from suit_dashboard.views import DashboardView


    class CustomView(DashboardView):
        pass

This view will render the base template from Django Suit Dashboard. If you
want to add CSS or JS libraries in your HTML, you can specify the ``template``
class variable in your view:

.. code:: python

    class CustomView(DashboardView):
        template_name = 'a/project/template.html'

In the template, you can override or overload the ``dashboard_css`` and
``dashboard_js`` blocks. Example:

.. code:: html

    {% extends "suit_dashboard/base.html" %}
    {% load static %}

    {% block dashboard_css %}
      {% if not suit %}
        <link href="{% static 'bower_components/bootstrap/dist/css/bootstrap.min.css' %}" rel="stylesheet">
        <link href="{% static 'bower_components/bootstrap/dist/css/bootstrap-theme.min.css' %}" rel="stylesheet">
      {% endif %}
    {% endblock %}

    {% block dashboard_js %}
      {% if not suit %}
        <script src="{% static 'bower_components/jquery/dist/jquery.min.js' %}"></script>
        <script src="{% static 'bower_components/bootstrap/dist/js/bootstrap.min.js' %}"></script>
      {% endif %}
      <script src="{% static "bower_components/highcharts/highcharts.js" %}"></script>
      <script src="{% static "bower_components/highcharts/modules/heatmap.js" %}"></script>
      <script src="{% static "bower_components/highcharts/highcharts-more.js" %}"></script>
    {% endblock %}

.. note::

    As you can see, a ``suit`` variable is available in the context of every
    view based on ``DashboardView``. You can use it to adapt your content for
    Django Suit and classic Django styles. In the example above, we use it
    to avoid adding Bootstrap a second time



Nested views and breadcrumbs
----------------------------

Nesting views is useful if you want to build a page tree in your admin
interface. It is also useful to build navigation breadcrumbs.

Let's play with an example:

.. code:: python

    # in your AdminSite class

    def get_urls(self):
        urls = super(DashboardSite , self).get_urls()
        custom_urls = [
            url(r'^$', self.admin_view(CustomView.as_view()), name='index'),
            url(r'nest1/^$', self.admin_view(NestView1.as_view()), name='nest1'),
            url(r'nest1/nest2/^$', self.admin_view(NestView2.as_view()), name='nest2'),
        ]

    # in your views

    class CustomView(DashboardView):
        template_name = 'a/project/template.html'
        crumbs = ({'url': 'admin:index', 'name': _('Custom')}, )


    class NestView1(CustomView):
        crumbs = ({'url': 'admin:nest1', 'name': _('Nest 1')}, )


    class NestView2(NestView1):
        crumbs = ({'url': 'admin:nest2', 'name': _('Nest 2')}, )


This set of views will automatically render a navigation bar like in the
following images (classic-styled and suit-styled):

.. image:: https://cloud.githubusercontent.com/assets/3999221/23990246/bed6ea70-0a35-11e7-95d3-bcc5e7904ba3.png
    :alt: Classic-style breadcrumbs
.. image:: https://cloud.githubusercontent.com/assets/3999221/23990258/c4286fd0-0a35-11e7-95eb-c439fdc68997.png
    :alt: Suit-style breacrumbs


Layout
======

Widgets
=======

Real-time widgets
-----------------

Templates
=========

Main template
-------------

Box templates
-------------

Widget templates
----------------

Suit menu
=========