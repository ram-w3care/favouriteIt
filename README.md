# Favourite It

Favourite any item you like.

## Requirements

This plugin requires Craft CMS 5.3.0 or later, and PHP 8.2 or later.

## Installation

You can install this plugin from the Plugin Store or with Composer.

#### From the Plugin Store

Go to the Plugin Store in your project’s Control Panel and search for “Favourite It”. Then press “Install”.

#### With Composer

Open your terminal and run the following commands:

```bash
# go to the project directory
cd /path/to/my-project.test

# tell Composer to load the plugin
composer require ram-suthar/craft-favourite-it

# tell Craft to install the plugin
./craft plugin/install favourite-it
```

### Features
- Supports Both Guests and Logged-in Users: Everyone can mark their favorites.
- Single-Click Favorite: Easily add any entry to favorites with a single click.

## Usage

#### Action
The plugin's primary functionality is accessible through the following action:
``` bash
actions/favourite-it/default/index
```
### Template Tags
The plugin provides the following template tags for use in your templates:
``` bash #### isItemFavourited(entryId) ```
Checks if the item with the specified ``` bash entryId ``` has been favorited.
##### Parameters:
- ``` entryId (required):``` The unique ID of the item to check.
##### Returns:
```  true ``` if the item is favorited, ```  false ``` otherwise.
``` bash
#### showFavourite()
```
Retrieves an array of all entry IDs that have been favorited.
Returns:
An array of entry IDs that have been favorited.

Here's an example of how to integrate it into your templates:

``` bash
{% set blogs = craft.entries().section('blog').all() %}
	{% for entry in blogs %}
	{% set featured = entry.featureImage.one() %}
	{% set postCategories = entry.postCategories.all() %}
	{% set itemExist = isItemFavourited(entry.id) %}
	<article>
	  <figure class="card-img-top m-0 overflow-hidden bsb-overlay-hover">
	    <a href="#!">
	      <img src="{{ featured.url }}" alt="{{ entry.title }}" />
	    </a>
	  </figure>
	  <h2>
	    <a href="{{ siteUrl('blog/entry.url') }}">{{ entry.title }}</a>
	  </h2>
	  <ul>
	    <li>
	      <button type="button" data-id="{{ entry.id }}" class="add_fav" data-csrf-token-value="{{ craft.app.request.getCsrfToken() }}">Add to Favourite</button>
	    </li>
	  </ul>
	</article>
  {% endfor %}
```
#### Using AJAX
For a smooth user experience, use AJAX to handle favorite actions:
``` bash
$(document).ready(function () {
        $(".add_fav").click(function () {
			let id = $(this).data("id");
      let button = $(this);
			const csrfToken = $(this).data("csrf-token-value");
            $.ajax({
              url: "{{ siteUrl('actions/favourite-it/default/index') }}",
              data: { id: id, CRAFT_CSRF_TOKEN: csrfToken },
              dataType: "json",
              type: "post",
              success: function (response) {
                if (response.status === 1) {
                  $(button).text("Remove from Favourite");
                }else if (response.status === 0) {
                  $(button).text("Add to Favourite");
                }
              },error:function(xhr, status, error){
                  window.location.reload();
              },
            });
        });
      });
