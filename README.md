# ExTTuPendoLous (alpha version - under construction)
Extended Tag Template - simple extended tag system for templating

## Description

A template system based into a macro language, where you can build your own macro blocks.

It works like C macro language, inserting text/data where a macro block appears, every macro start with
a at symbol (@), a ':' can be used to continue blocks if their result is true, per default any macro blocks
should return true, a false result will not execute any consecutives macro blocks.

## Macro blocks

Macro block start with a optional name and open/close symbol, blocks accept any kind of text. The format
of the text can be defined by developer.

## Default context

This library works with a context class (include/extpl/context.hpp) that implements a class to access symbols, 
symbols are responsible to validate and run a code block, the default context (ExTPL::Context::Default) runs
a JavaScript VM ([duktape.org]) and symbols run on it.

Ps.: Default context is very simply and will be refined with time.

## Macro blocks examples

```
@defs{
	// default vars value (will not overwrite existent vars)
	// must be in a JavaScript Object Format (JSON like),
	// that will run by JS VM
	// this block need to end with a } after a newline (\n)
	title: 'Home',
	content: 'index'
}

@vars{
	// set vars (will overwrite existent vars)
	// this block need to end with a } after a newline (\n)
	title: 'Home',
	content: 'index',
}

@js{
	// js code to run
	// this block need to end with a } after a newline (\n)
	$.print("}"); // this passes
}

@["include_file"]
@["include_file.js"]
@{"print this text"}
@{var_to_print}

@rm{
	Comment/Descarded block
	this block need to end with a } after a newline (\n)
}

@if(
	// js expression
	// this block need to end with a ) after a newline (\n)
	$.vars.i == 10 && $.vars.j == 20
):{"text"}

@exists(var_name):{"<div>"}:{var_name}:{"</div>"}

@exists(var_name):js{
	
}

@["file1.js"]:js{
	// js code..
}
```

## To Do

* put node-extpl in a separated repository
* put ExTPL::Context::Default into a separated library
* create a best text filter/transform mechanism (ExTPL::TextFilter maybe?)
* API Documentation (soon)

## Example

```html
@rm{
	File: test.html.xtpl
	Description: This is a example of template file
}<!DOCTYPE html>
<html>
<head>
	<title>@{title}</title>
</head>
<body>
@[content]
</body>
</html>
```