# ScrivitoAdvancedEditors

Provides some advanced editors and partials for Scrivito's details views.

## Installation

Add to your gemfile:

    gem 'scrivito_advanced_editors'

Add this line to your application stylesheet manifest:

    *= require scrivito_advanced_editors

Add this line to your JavaScript manifest:

    //= require scrivito_advanced_editors

## New in Version 1.4.x

- Color picker can use lists of lists for better color structuring
- Added partial for preview buttons

## Included Editors

- [Scrivito Toggle Button Editor](#toggle_button)
- [Scrivito Multi-Selection Button Editor](#multi_button)
- [Date Time Editor](#date_time)

### <a id="toggle_button"></a>Scrivito Toggle Button

This editor changes Scrivito's normal `enum` editor to a toggle button editor.

#### Usage

Activate this editor in your editor selection:

```ruby
scrivito.select_editor(function(element, editor) {
  editor.use("toggle_button_editor");
});
```
Using this editor with `enum` attributes works automatically.

```ruby
# in details view my_class/details
scrivito_tag(:div, @obj, :my_enum_attribute)
```

You can also use this editor with string elements. In this case, just pass the list to the `scrivito_tag` helper:

```ruby
scrivito_tag(:div, @obj, :my_string_attribute, data: {toggle_button_list: ['elem1', 'elem2', 'elem3']})
```

If you require localisation for your editors or want to provide more details to a single button, you can set a data attribute:

```ruby
scrivito_tag(:div, @obj, :my_enum_attribute, data: {toggle_button_caption: {'elem1' => 'caption1', 'elem2' => 'caption2', 'elem3' => 'caption3']})
```

This list does not need to be complete. If you do not provide a caption for an element, the source element is used.

Normaly the toggle button editor can activate and deactivate the selection. In some use cases a deactiavtion ist not wanted. Then you can at the parameter `data-scrivito-toggle-button-enable=false` to change this behavior.

```ruby
scrivito_tag(:div, @obj, :my_enum_attribute, data: {scrivito_toggle_button_enable: false})
```

### <a id="multi_button"></a>Scrivito Multi-Selection Button

This editor changes all `multienum` fields to buttons.

#### Usage

Activate this editor in your editor selction:

```ruby
scrivito.select_editor(function(element, editor) {
  editor.use("toggle_multi_select_editor");
});
```

Using this editor with `multienum` attributes works automatically:

```ruby
# in details view my_class/details
scrivito_tag(:div, @obj, :my_enum_attribute)
```

You can also use this editor with string elements. In this case, just pass the list to the `scrivito_tag` helper:

```ruby
scrivito_tag(:div, @obj, :my_string_attribute, data: {multi_select_list: ['elem1', 'elem2', 'elem3']})
```

If you require localisation for your editors or want to provide more details to a single button, you can set a data attribute:

```ruby
scrivito_tag(:div, @obj, :my_string_attribute, data: {multi_select_caption: {'elem1' => 'caption1', 'elem2' => 'caption2', 'elem3' => 'caption3']})
```

This list does not need to be complete. If you do not provide a caption for an element, the source element is used.

### <a id="date_time"></a>Date Time Picker

This editor changes all `date` fields to a better date time picker. It is using [FlatPickr](https://github.com/chmln/flatpickr)

#### Usage

Activate this editor in your editor selction:

```ruby
scrivito.select_editor(function(element, editor) {
  editor.use("datetime_editor");
});
```

This editor porvides some conig operators. These can be defined in the data attribute `data-scrivito-datetimepicker-config`:

```ruby
  scrivito_tag(:div, obj, :my_date_attribute, data: {scrivito_datetimepicker_config: {
    dateFormat: 'Y-m-d', # The Format the date will be presented
    enableTime: true, # Enable or diable the time picker
    enableSeconds: true, # Enable or disable the seconds in the time picker
    hourIncrement: 1, # Using keys or scroll steps for hours
    minuteIncrement: 5, # Using keys or scroll steps for minutes
    minDate: null, # Min date can be a valid string or date object
    maxDate: null, # Max date can be a valid string or date object
    time_24hr: true, # 24 hour time format
    inline: false, # presents the picker inline directly under the input field
    weekNumber: false, # shows week numbers
    allowInput: false, # allows the user to type the date in the input field
    disable: [], # List of dates the are not usable
    enable: [] # List of dates that are only usable
  }})
```

Examples for enabling or disabling dates:

```JS
  // List of dates
  {
    disable: ["2017-03-30", "2017-05-21", "2017-06-08", new Date(2017, 8, 9) ]
  }
```

```JS
  // disable with a range
  { disable: [
      {
        from: "2017-04-01",
        to: "2017-06-01"
      },
      {
        from: "2017-09-01",
        to: "2017-12-01"
      }
    ]
  }

```

```JS
  // use a function
  { "disable": [
      function(date) {
        return (date.getDay() === 5 || date.getDay() === 6); // hide weekends
      }
    ],
    "locale": {
      "firstDayOfWeek": 1 // start week on Monday
    }
  }
```


## Included Extensions

- [Create New Obj](#new_obj)
- [Inline Help](#inline_help)
- [Tabs](#details_tabs)
- [Accordion](#details_accordion)
- [Color Picker](#color_picker)
- [Preview Buttons](#preview_buttons)
- [Bind toggle button editor to tab (Beta)](#bind_tab_toggle)
- [Adding a colapsable (Beta)](#add_colapsable)

### <a id="new_obj"></a>Button for creating a new CMS object

It could be helpful to have a button for creating a new CMS object like a blog post.
To use the script, add a form that uses the `create-obj`  CSS class. Also, add `data-obj-class` and `data-obj-path` as attributes:

```xml
<form class="create-obj" data-obj-class="BlogPost" data-obj-path=@obj.path data-with-date="created_at">
  <button>
    Create a new Blog Post
  </button>
</form>
```

For this simple example, you can also use a partial provided by this gem.

    <%= render "scrivito_advanced_editors/create_obj", obj_class: "BlogPost", obj_path: @obj.path, with_date: "created_at" %>

`with_date` optionally specifies a date attribute.

### <a id="inline_help"></a>View helper for explanations in details views

Some attributes are hard to describe in short. To provide an editor with a more detailed description, you can add alerts to your view. This feature requires `bootstrap` to be enabled in your app.

```html
<div class="alert alert-info">Do it in this way or look here</div>
```

To offer help unobtrusively, a small icon next to the headline can be used. For this, an `abbr` tag is suitable:

```html
<h4>My attribute <abbr title="My long description" class="initialism"></abbr></h4>
```

By default, we render a "?" as content using CSS, but you can change this in accordance with your own styles:

```css
.details-view abbr:after {
  content: " more details"
}
```

Another option is to use `small`:

```html
<h4>My Attribute <small>This is a description for my attribute</small></h4>
```

### <a id="details_tabs"></a>Tab View

The purpose of the tab view is to separate your attributes. This is useful if your object contains many of them, such as meta data, data for different devices, and so on.

#### Usage

To utilize the tab view, please add some blocks to your details view.
Use the `active` class to select the initially active panel of the tab list:

```xml
<ul class="tab-list">
  <li data-panel-target="#panel1" class="active">Elem1</li>
  <li data-panel-target="#panel2">Elem2</li>
  <li data-panel-target="#panel3" class="inactive">This is an inactive element</li>
</ul>

<div class="tab-panels">
  <div class="tab-panel" id="panel1">
    scrivito_tags for panel 1 are here ....
  </div>

  <div class="tab-panel" id="panel2">
    scrivito_tags for panel 2 are here ....
  </div>

  <div class="tab-panel" id="panel3">
    scrivito_tags for panel 3 are here ....
  </div>
</div>
```

Using the `inactive` class for a list element removes the click handler and makes the font color brighter. Conditionally enabling and disabling click handlers lets you enable or disable panels depending on the values of other elements.

You can also define the tab to have a vertical style. Simply add the css class `vertical` to your tab list.

```xml
<ul class="tab-list vertical">
  ...
</ul>

...
```

### <a id="details_accordion"></a>Accordion

The accordion can be used to separate your attributes into groups or hide them if they are too space-consuming.

It is also helpful for rendering a `widgetlist` in a details view. A tab widget could be a good example for this. You can use the accordion view in your panels and render them in details view of the tab widget.

#### Usage

To utilize the accordion, please add some blocks to your details view.

```xml
  <div class="scrivito-title-details">
    <%= scrivito_tag :h4, widget, :title %>
    <%= link_to '(details)', '#', 'data-scrivito-toggle-details' => widget.id %>
    <div class="scrivito-details-<%= widget.id %>">
      ... Content for your panel ...
    </div>
  </div>

  ... next panel ...
```

### <a id="color_picker"></a>Color Picker

First, activate the editor in your editor selection callback:

```js
scrivito.select_editor(function(element, editor) {
  editor.use("color_picker");
});
```

To make the color picker available, use `scrivito_tag` and provide it with a color list:

```ruby
scrivito_tag :div, @obj, :background_color, data: {colors_list: ['red', 'green', 'blue']}
```

Then specify the CSS for the colors:

```css
.red {
  background_color: red !important;
}

.green {
  background_color: green !important;
}

// Also more styles can be defined, like text color if background is to dark.
.blue {
  background_color: blue !important;
  color: #fff !important;
}
.blue * { color: #fff; }
```

A better structure of data can be set separating them in arrays. This can be used to differentiate the colors in main and helping colors or define base colors and lighter and darker versions of them:

```ruby
scrivito_tag :div, @obj, :background_color, data: {colors_list: [['red-light', 'green-light', 'blue-light'],['red', 'green', 'blue'],['red-dark', 'green-dark', 'blue-dark'],nil,['transparent','white','gray','black']]}
```

Using nil will as list will generate a *hr*-tag.

You can also add the `data-scrivito-color-picker-show-text` attribute and set it to `true` to show a short text to visualize the text style for a color.

```ruby
scrivito_tag :div, @obj, :background_color, data: {
  colors_list: ['red', 'green', 'blue'],
  scrivito_color_picker_show_text: true
}
```

### <a id="bind_tab_toggle"></a>Bind toggle button editor to tab (Beta)

Sometimes there are attributes that are not useful in any situation. For example meta-data for Movies. If you want to hide some attributes, and toggle its accessibility. Scrivito Advanced Editors gem provides a binding between its tabs and toggle button editor.

We have a obj calss with a type attribute and different attributes for every type:

```ruby
def myClass < Obj
  attribute :type, :enum, values: ['Page','Movie','TvShow','Audio']

  # some attributes for special types
  attribute :page_name
  attribute :movie_title, :string
  attribute :tv_show_name, :string
  attribute :audio_type, :string
  ...
end
```

In details view for this obj we can add the toggle button to edit the type and a tab to categorize the attributes by type:

```xml
  <%= scrivito_tag :div, @obj, :type, data: {tab_to_toggle: "#typ_toggle_tab", scrivito_toggle_button_enable: false } %>

  <ul class="tab-list" id="typ_toggle_tab">
    <li data-panel-target="#panel1" data-allowed-values=["Page"]>Page</li>
    <li data-panel-target="#panel2" data-allowed-values=["Movie","TvShow"]>Movie</li>
    <li data-panel-target="#panel3" data-allowed-values=["Audio"]>Audio</li>
    <li data-panel-target="#panel4">Allways active</li>
    <li data-panel-target="#panel5" data-allowed-values="">Never active</li>
  </ul>

  <div class="tab-panels">
    <div class="tab-panel" id="panel1">...</div>
    <div class="tab-panel" id="panel2">...</div>
    <div class="tab-panel" id="panel3">...</div>
    <div class="tab-panel" id="panel4">...</div>
  </div>
<% end %>
```

With the `data-tab-to-toggle` parameter at the `scrivito_tag` for the type we activate the binding to a specific tab. To specify which tab is active add the parameter `data-allowed-values` to every panel. If this is not specified, the tab is allways active. If the value is empty, it will never be active.

No need for `class=active`. It is caluclated on the loading of the details view.

### <a id="preview_buttons"></a>Preview Buttons

Buttons to open a page from your details view. It is very helpful in the content browser to open the selected page.

```xml
<%= render 'scrivito_advanced_editors/page_preview_buttons', obj: @obj %>
```

### <a id="add_colapsable"></a>Adding a colapsable

One more method to hide Attributes or text. An easy colapsable:

```xml
<%= scrivito_colapsable_for 'Some hidden attributes' do %>
  ...
<% end %>
```

You can one on the predifined types:

```xml
<%= scrivito_colapsable_for 'Editing', 'editing' do %>
  ...
<% end %>
<%= scrivito_colapsable_for 'Danger', 'danger' do %>
  ...
<% end %>
<%= scrivito_colapsable_for 'Warning', 'warning' do %>
  ...
<% end %>
<%= scrivito_colapsable_for 'Help', 'help' do %>
  ...
<% end %>

<%= scrivito_colapsable_for 'Mark', 'mark' do %>
  ...
<% end %>
<%= scrivito_colapsable_for 'Small', 'small' do %>
  ...
<% end %>
```

`editing` is the default. The types `mark` and `small` can combined together with the other ones:

```xml
<%= scrivito_colapsable_for 'Mark', 'warning mark' do %>
  ...
<% end %>
<%= scrivito_colapsable_for 'Small', 'help mark small' do %>
  ...
<% end %>
```

If you has Bootstrap in your details view available, you can also use bootstraps helper classes like centering text or setting background colors.

You cann add your own type easily with css:

```xml
<%= scrivito_colapsable_for 'Attributes', 'my_type' do %>
  ...
<% end %>
```

```css
.scrivito_colapsable_for_container.my_type .scrivito_colapsable_editing {
  color: green;
}
```
