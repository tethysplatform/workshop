---
title: Getting Started
---
Getting Started
===============

## Scaffold a New App

To begin developing a new Tethys app, you should start with a Tethys scaffold. This will include all the initial files you'll need to get an app up and running.

Start by activating your virtual environment, and execute the following commands to create a new directory to put your Tethys app inside:

```bash
# Create a working directory (if it doesn't exist already)
mkdir ~/tethysdev
cd ~/tethysdev
```

Next, scaffold the app by running:

```bash
tethys scaffold earthquake_calculator
```

You will be prompted to enter metadata about the app such as, proper name, version, author, and description. All of these metadata are optional and can be changed later in the generated app.py file (as you'll note later on). You can accept the default values by pressing enter, repeatedly.

In a file browser change into your Home directory and open the tethysdev directory. If the scaffolding worked, you should see a directory called tethysapp-map_layout_tutorial. All of the source code for your app is located in this directory.

To test your newly scaffolded app, install it by running the following commands:

```bash
cd tethysdev/tethysapp-earthquake_calculator
tethys install -d
```

Now, go to [localhost:8000](http://localhost:8000) to view your app in your browser.

## Customize Your App
Now that you have your app running, let's customize it.

Some ways you can change the look of your app are:
- App title (shows up in top left corner)
- Icon
- Color

To change these properties of your app, open the app.py file in your app files.

You'll find something that looks like this:

```python
class App(TethysAppBase):
    """
    Tethys app class for Earthquake Calculator
    """

    name = 'Earthquake Calculator'
    package = 'earthquake_calculator'  # WARNING: Do not change this value
    index = 'home'
    icon = f'{package}/images/icon.gif'
    root_url = 'earthquake-calculator'
    color = '#003087'
```

### App Title


If you'd like to change the name that appears in the Tethys portal for your app, change the 'name' line to whatever you'd like. For example, if you changed it to 'My Brand New App', like so:

```python
name = 'My Brand New App'
```

You would then see this in the app library:

*** INSERT APP LIBRARY SCREENSHOT HERE ***

And if you go into your app, you would see something like this:

*** INSERT APP SCREENSHOT HERE ***

### App Icon

To set the image that shows up for your app in the Tethys library and next to the app name, first add that image to the `public/images` directory inside your app files.

Then, change the icon attribute to the file name like so: 

```python
icon = f'{package}/images/new_icon.png'
```

### App Color

To change the accent color used throughout your app, change the color to any hexcode color you'd like:

```python
color='#bb323d'
```

After making these changes, just reload your app and you'll see them reflected in the browser inside your app.

## Customize Your Home Page

Now, let's try changing the contents of your home page by editing the contents of `templates/home.html`.

For this tutorial, you'll be building a simple app that performs calculations relating to earthquake data. 

For this example, we'll have the homepage of your app be a page wuth some introductory information.

Remove everything inside `home.html` and paste in the following contents:


```html
{% extends tethys_app.package|add:"/base.html" %}
{% load tethys %}

{% block app_content %}
  <div class="container">
    <h1>Earthquake App</h1>
    <p>This is the home page of the Earthquake App.</p>

    <p>In this app, you can perform a powerful analysis of an earthquake's data. </p>
    <a href="calculate"><button class="btn btn-secondary">Go to Calculations Page</button></a>
  </div>

{% endblock %}

```

Now reload the app in your browser and you should see your introductory information and a button below.

## Add a New Page

Next, we'll make it so that when a user presses that "Go to Calculations Page" button, they'll be directed to a new page.

Begin by adding a new template file inside `templates/earthquake_calculator` called `calculations.html`. 

Paste the following into this file:

```html
{% extends tethys_app.package|add:"/base.html" %}
{% load tethys %}

{% block app_content %}
<p>This is the calculations page</p>
{% endblock %}
```

Next, we'll need to add a new controller function to render this new page. Inside `controllers.py` add the following code:

```python
@controller(name="calculate", url='calculate')
def calculate(request):
  return App.render(request, 'calculations.html')
```

Next, navigate to 

localhost:8000/apps/earthquake_calculator/calculate

You should see something like this:

*** INSERT SCREENSHOT HERE ***

## Add Calculation Inputs
Now, let's look at putting some gizmos into the Calculations Page

Gizmos are building blocks that can be used to create beautiful interactive controls for web apps. Using the Template Gizmos API, developers can add date-pickers, plots, and maps to their app pages with minimal coding.

### Define Gizmos in Controller
We begin by adding these gizmos to our controller:

```python

@controller(name="calculate", url='calculate')
def calculate(request):
    primary_time_difference1 = TextInput(name='primary_time_difference1', display_text='First Primary Time Difference (seconds)', placeholder='e.g. 10', attributes={'type': 'number'})
    secondary_time_difference1 = TextInput(name='secondary_time_difference1', display_text='First Secondary Time Difference (seconds)', placeholder='e.g. 20', attributes={'type': 'number'})
    primary_time_difference2 = TextInput(name='primary_time_difference2', display_text='Second Primary Time Difference (seconds)', placeholder='e.g. 10', attributes={'type': 'number'})
    secondary_time_difference2 = TextInput(name='secondary_time_difference2', display_text='Second Secondary Time Difference (seconds)', placeholder='e.g. 20', attributes={'type': 'number'})
    calculate_button = Button(display_text='Calculate', name='calculate_button', icon='fa fa-calculator', submit=True, attributes={'class': 'btn btn-primary'})

    context = {
        'primary_time_difference1': primary_time_difference1,
        'secondary_time_difference1': secondary_time_difference1,
        'primary_time_difference2': primary_time_difference2,
        'secondary_time_difference2': secondary_time_difference2,
        'calculate_button': calculate_button}

    return App.render(request, 'calculations.html', context)
```

### Add Gizmos to Template

Now that you've defined these gizmos, you'll need to add them to your page inside the template. Open `calculations.html` and add the following: 

```html
{% block app_content %}
  {% gizmo primary_time_difference1 %}
  {% gizmo secondary_time_difference1 %}
  {% gizmo primary_time_difference2 %}
  {% gizmo secondary_time_difference2 %}
  {% gizmo calculate_button %}
{% endblock %}

```

After refreshing the page in your browser, you should see the form with 4 inputs and a "Calculate" button. For now, the calculate button won't actually calculate anything. We'll change that soon.

## Update Base Template

For now, we are using the default version of what is called a base template. That template provides all of this:

*** INSERT SCREENSHOT** 

We can change that template, however. For our calculation page, we're going to use a base template that provides two columns with no navigation bar on the side.

Change the top line of your `calculations.html` file to this: 

```html
  {% extends "tethys_apps/app_two_columns.html" %}
```
Next, let's place the calculation inputs in the left column of your page.

Update the name of the app_content block to:

```html {1}
{% block app_content_lc %}
  {% gizmo primary_time_difference1 %}
  {% gizmo secondary_time_difference1 %}
  {% gizmo primary_time_difference2 %}
  {% gizmo secondary_time_difference2 %}
  {% gizmo calculate_button %}
{% endblock %}
```

While we're at it, let's add a bit of content to the right column so you can visualize how these columns will look. 

Add the following to `calculations.html`:

```html 
{% block app_content_rc %}
  <p>This is the right column. You'll put the results of the calculation here.</p>
{% endblock %}
```

Go ahead and refresh your page in the browser, and you'll see this:

*** INSERT SCREENSHOT HERE ***

## Add Form Submission
Now, let's get your calculate button working. 

For that we'll need to place your inputs into a `<form>`. This will allow the page to send the inputs' data together to your view to run the calculation and return data. 


### Add a Form
Begin by making the following changes to `calculations.html`:

```html {2-3,9}
{% block app_content_lc %}
    <form method="post">
        {% csrf_token %}
        {% gizmo primary_time_difference1 %}
        {% gizmo secondary_time_difference1 %}
        {% gizmo primary_time_difference2 %}
        {% gizmo secondary_time_difference2 %}
        {% gizmo calculate_button %}
    </form>
{% endblock %}
```
### Update Controller
Now, let's update the controller to handle a POST request that will come from this form.


```python {5-29}
@controller(name="calculate", url='calculate')
def calculate(request):
    ...

    if request.method == "POST":
        primary_velocity = App.get_custom_setting('primary_velocity')
        secondary_velocity = App.get_custom_setting('secondary_velocity')

        primary_time_difference1_value = float(request.POST.get('primary_time_difference1'))
        secondary_time_difference1_value = float(request.POST.get('secondary_time_difference1'))
        primary_time_difference2_value = float(request.POST.get('primary_time_difference2'))
        secondary_time_difference2_value = float(request.POST.get('secondary_time_difference2'))

        distance1 = calculate_distance(
            primary_time_difference1_value,
            secondary_time_difference1_value,
            primary_velocity,
            secondary_velocity
        )

        distance2 = calculate_distance(
            primary_time_difference2_value,
            secondary_time_difference2_value,
            primary_velocity,
            secondary_velocity
        )

        print("Distance 1: ", distance1)
        print("Distance 2: ", distance2)

    return App.render(request, 'calculations.html', context)
```

Now go ahead and add values to the 4 inputs in your calculation page, and press the calculate button. You should see values showing up in your terminal.

### Display results in page
Now, let's update your calculation results side to show the results in your calculation page:

Begin by adding the distances to your context that will be passed into your template from your calculation controller:

```python {20-21}
@controller(name="calculate", url='calculate')
def calculate(request):
    ...
    if request.method == "POST":
        ...

        context["distance1"] = distance1
        context["distance2"] = distance2

    return App.render(request, 'calculations.html', context)

```

Now, let's render those results in the template. This template will render regardless if the post request has been sent, so we'll want to only show the results after we've checked if the context includes them.

```html {2-7}
{% block app_content_rc %}
  {% if distance1 and distance2 %}
      <p>Distance 1: {{ distance1 }}
      <p>Distance 2: {{ distance2 }}
  {% else %}
      <p>Run the calculation to the left to see data here.</p>
  {% endif %}
{% endblock %}
```

Now go ahead and refresh the page, and test the calculations again. You should see them show up in the right column of your page like so:
*** insert screenshot ***

## Add Plot Graphic

Now that the calculations are working, let's display them in a nicer-looking way. For that, we'll use the BarPlot gizmo. 

Begin by once again defining the gizmo in your controller:

```python {7-23}
@controller(name="calculate", url='calculate')
def calculate(request):
    ...
    if request.method == "POST":
        ...

        bar_plot = BarPlot(
            engine='highcharts',
            title='Distances from Epicenter',
            subtitle='Distances',
            vertical=True,
            axis_units='km',
            axis_title='Distance from epicenter',
            series=[{
                'name': 'Station 1',
                'data': [distance1]
            }, {
                'name': 'Station 2',
                'data': [distance2]
            }]
        )

        context['bar_plot'] = bar_plot

    return App.render(request, 'calculations.html', context)
```

Now, let's add this bar plot to your page:

```html {2-3}
{% block app_content_rc %}
  {% if bar_plot %}
      {% gizmo bar_plot %}
  {% else %}
      <p>Run the calculation to the left to see data here.</p>
  {% endif %}
{% endblock %}
```

Go ahead and refresh your page, and run your calculations one more time. This time, you should see a beautiful graph pop up with your calculation results. 

*** INSERT SCREENSHOT HERE ***

That's it! You've developed a working Tethys app with an earthquake calculator. 
