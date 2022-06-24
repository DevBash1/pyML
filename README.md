# pyML
pyML (Python Markup Language) is a markup language that uses Python Similar Syntax and Indentation Style.   
pyML is interpreted to HTML code.

# Install
pyML can be installed with npm using the command below.

```
npm i -g pyml
```

# Usage

After Installing pyML,   
Create a directory for your new pyML Project and cd into the directory.   
Then run pyML on the directory with parameter '-s' to spinup a local server on port 4200.

```
mkdir mypymlapp
cd mypymlapp
pyml -s
```

pyML takes 2 parameters

```
pyml <directory> -s
```

omiting the directory will make pyML run on the current directory.
'-s' makes pyML run a special server with live reload feature.

When pyml is running.
It keeps watching for .pyml files and once a .pyml file is modified,   
pyML will create a .html file corresponding to the .pyml file name and interpret the pyML code to HTML code.   

This means,   
if a file named 'index.pyml' is modified,   
pyML creates a file named 'index.html' or overides the file if it already exists.

# pyML

pyML's syntax is very simple and straight forward.
I will use a pyML and HTML example to explain the syntax.

## Tag
To create a tag in pyML we only use the name of the tag eg div,br,h1,h2,etc.   
It does'nt matter if it's a Single Tag or not.
And we use indentation for organizing tags for knowing parent tags and child tag.

pyML
```python
html
    title > My Web Site
    body
        h1 > My Website
        hr
        h1 > Welcome to my Website
        br
        h1 > How can i help you?
```
HTML
```html
<html>
  <title>My Web Site</title>
  <body>
    <h1>My Website</h1>
    <hr>
    <h1>Welcome to my Website</h1>
    <br>
    <h1>How can i help you?</h1>
  </body>
</html>
```

## innerHTML
To add an innerHTML to a tag or content of a tag we use a (Greater Than Symbol) '>' followed by a space and then the content of the tag.   
Eg   

pyML
```python
html
    title > My Web Site
    head
        script src="script.js"
        link rel="stylesheet", href="style.css"
    body > Hello World!
```
HTML
```html
<html>
  <title>My Web Site</title>
  <head>
    <script src="script.js"></script>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>Hello World!</body>
</html>
```

We can also add muiltiline innerHTML content.

pyML
```python
html
    body
        h1 > /
        Am a muiltiline text!
        Am a muiltiline text!
        Am a muiltiline text!
        /
```

HTML
```html
<html>
  <body>
    <h1>
      Am a muiltiline text!
      Am a muiltiline text!
      Am a muiltiline text!
    </h1>
  </body>
</html>
```

pyML
```python
html
    body
        h1 > /
        <b>Am a muiltiline text!</b>
        <b>Am a muiltiline text!</b>
        <b>Am a muiltiline text!</b>
        /
```

HTML
```html
<html>
  <body>
    <h1>
      <b>Am a muiltiline text!</b>
      <b>Am a muiltiline text!</b>
      <b>Am a muiltiline text!</b>
    </h1>
  </body>
</html>
```

We use a '/' after '>' to start a muiltiline text and also ends it on a line with '/'.
'/' can be escaped with another '/' to make pyML not stop the muiltiline there.

pyML
```python
html
    body
        h1 > /
        <b>Am a muiltiline text!</b>
        <b>Am a muiltiline text!</b>
        <b>Am a muiltiline text!</b>
        //
        <b>Am a muiltiline text!</b>
        <b>Am a muiltiline text!</b>
        <b>Am a muiltiline text!</b>
        /
```
HTML
```html
<html>
  <body>
    <h1>
      <b>Am a muiltiline text!</b>
      <b>Am a muiltiline text!</b>
      <b>Am a muiltiline text!</b>
      /
      <b>Am a muiltiline text!</b>
      <b>Am a muiltiline text!</b>
      <b>Am a muiltiline text!</b>
    </h1>
  </body>
</html>
```

**Note:** Having an innerHTML does not mean a tag can not have children.

pyML
```python
html
    body > Am a parent!
        p > Am a child!
        br
        p > Am a child!
```

HTML
```html
<html>
  <body>Am a parent!
    <p>Am a child!</p>
    <br>
    <p>Am a child!</p>
  </body>
</html>
```

## Attributes
To add an attribute to a tag or attributes,   
We need to specify them after the tag name in this format.

```
tagname attribute="value", attribute2="value", attribute3="value" > innerHTML
```

For this to work,   
We have to use double quotes for the values and seperate the attributes with a comma.   
We can not use string escaping in the value.   
We also can not have only attributes and no value.   

In HTML,   
We can have tags like.
```html
<button id="login" onclick="login()" disabled>Login</button>
```
As you can see from the HTML code above,   
There is a single attribute 'disabled' in the HTML code.   
This can not be done in pyML,   
You have to provide the value like in code below.
```python
button id="login", onclick="login()", disabled="true" > Login
```

If any error is detected in a tag's attribute,   
The tag will be ignored by pyML.

## Special Tags
pyML has some special tags you can use to do special things.   

### @comment
@comment just like the name implies is used to add comments to our pyML code.   
**Note:** This comments will not be interpreted to HTML comments.

pyML
```python
html
    body
        @comment("Body Content Goes here")
        h1 > Hello World!
```

HTML
```html
<html>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

### @param
@param is used to declare key to value type of variable.   
@param only accepts string as key and value.    
It is used in conjunction with **@import**.

pyML
```python
@param("name","John Doe")
```

### @import
@import is used to import another .pyml code into the current code.  
You can have .pyml codes as components of your HTML UI and import them and reuse.   
Example

index.pyml
```python
html
    body
        h1 > Hello, Visitor
        @param("brand","Shopify")
        @param("promocode","CLOTH2022")
        @import("welcome.pyml")
```

welcome.pyml
```python
h1 > Welcome to {{brand}}
div
    h2 > {{brand}} got you covered?
    h3 > get good clothes for a good prize only on {{brand}}.
    p > Use The Promo code 
        b > {{promocode}}
```

index.html
```html
<html>
  <body>
    <h1>Hello, Visitor</h1>
    <h1>Welcome to Shopify</h1>
    <div>
      <h2>Shopify got you covered?</h2>
      <h3>get good clothes for a good prize only on Shopify.</h3>
      <p>Use The Promo code <b>CLOTH2022</b></p>
    </div>
  </body>
</html>
```
