# Flickr in your Pelican website
This [Pelican] plugin brings your Flickr photos & sets into your static website.

## Install
*TODO pip*

Add the plugin path to your **PLUGINS** setting in the pelicanconf.py file.
```
PLUGINS = ['pelicanflickr', ]
```
You must setup at least two settings for this plugin to work (see Settings section for more details):
 * FLICKR_API_KEY
 * FLICKR_USER

Finally, you can run ```pelican``` to render your website.

For the first run, as the cache is empty and can be quite long to populate i recommend using ```pelican --debug``` to see what is happening...

## Settings
The following settings should be specified in your pelican configuration file, usually ```pelicanconf.py```

### FLICKR_API_KEY
This setting is **mandatory**.

If you don't already have one, you must ask for a Flickr [Api key] (it's free). Here we only need the key, not the secret part used only for authentified actions.

### FLICKR_USER
This setting is **mandatory**.

Use [idGettr] to find your Flickr id, it should look like ```XXXXXXXX@YYY```

### FLICKR_OUTPUT_DIRNAME
This setting is optional, its default value is ```flickr```.

This setting sets the name of the output directory for all the files generated by this plugin (sets & photos). It will be a part of the urls on your website.

### FLICKR_UPDATE
This setting is optional, its default value is ```True```.

To speed up cache usage & overall rendering we can forbid the usage of the Flickr API once the cache has been built, by setting it to ```False```.

### FLICKR_CACHE
This setting is optional, its default value is ```True```.

This is a bit the opposite effect of FLICKR_UPDATE, as it forbids the usage of the cache when sets to ```False```. 

Essentially for development & testing purposes.

### FLICKR_SETS_EXCLUDE
This setting is optional, its default value is ```None```.

You can specify a list of Flickr sets id or name, to exclude them on your website.

### Example
My config looks like this:
```python
FLICKR_API_KEY = 'xxxXXXxC0FFEE'
FLICKR_USER = '123456789@YYY'
FLICKR_SETS_EXCLUDE = ['Compromising pictures', ]
FLICKR_OUTPUT_DIRNAME = 'photos'
FLICKR_UPDATE = False
```

## Templates

### General context (any page)
You can access all your Flickr photosets from any generated page using the ```flickr_sets``` variable as in the example below.
```html
{% for set in flickr_sets %}
<div class="set">
    <a class="primary" href="{{ set.url }}">
        <img class="light" src="{{ set.primary.sizes.medium.source }}" />
        {{ set.title }}
    </a>
</div>
{% endfor %}
```

### Photosets
Every photoset available will have a file generated in the ```FLICKR_OUPTUT_DIRNAME```

A variable named **photoset** is added to this page's context.
This plugin embeds a default template ```flickr_set.html``` that you can override by creating a file in your template dir with the same name.

Here is basically the content of the default file:
```html
<h1>{{ photoset.title }}</h1>
{% for photo in photoset.photos %}
  <a href="{{photo.url}}">
    <img src="{{photo.sizes.largesquare.source}}" title="{{photo.title}}" />
  </a>
{% endfor %}
{% endblock %}
```

### Photos
As for the photo sets, each available photo generates a page, in a subfolder per photoset of ```FLICKR_OUPTUT_DIRNAME``` (ie. output/flickr/my-set/425169.html for the photo with id 425169).

Several variables are added to the page context:

 * ```photoset``` is the parent photo set object
 * ```photo``` is the current photo object
 * ```photo_previous``` is the possible previous photo object in the parent set (may be null)
 * ```photo_next``` is the possible next photo object in the parent set (may be null)

You can override the default ```flickr_photo.html``` by adding a file with the same name in your template dir.

[Pelican]: http://getpelican.com
[Flickr]: http://flickr.com
[idGettr]: http://idgettr.com/(env)
[Api key]: https://www.flickr.com/services/apps/create/apply
