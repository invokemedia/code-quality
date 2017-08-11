Code Formatting & Quality Rules
===============================

### Why?

In order to help reduce cognitive overhead when joining a new project started by another developer, we apply coding standards and formatting to all our code.

If everyone on the team can get on board with a particular style, we can make it easier to pass projects around and contribute to each others work.

## Git Hooks

You can use git hooks in a project to make sure that certain actions are always run before you commit any code. To do this, you need to add a script to a special folder in your git project.

Here is an example:

```
touch .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

This creates a new file under `.git/hooks/pre-commit` and then enabled execution on that file.

If you do you the `pre-commit` hook, you will be unable to commit any code unless the script runs with `exit 0`. If there are any exit codes other than `0`, git will block you from committing.

You can see an example of git hooks in use and how to write one using PHP `composer` under our [php-starter](https://github.com/invokemedia/php-starter) repo.

## Tools

The standards outlined here are broken down into 3 groups: formatting, linting, and testing.

### PHP

PHP has a community suggestions around different coding practices and standard. These are called *PHP Standard Recommendations* or *PSRs*.

#### Formating

Formatting for PHP code should be in the format described in [PSR-2](http://www.php-fig.org/psr/psr-2/). You should install and setup [PHP Code Sniffer](https://github.com/squizlabs/PHP_CodeSniffer) in your editor of choice and set the tool *to run on save*. You should make sure that the `--standard=PSR2` flag is always used when the command is run.

If you want to run the program from the command line, use the following command:

```
phpcs -n --standard=PSR2 --extensions=php --colors path/to/code
```

There is also a tool called [PHP CS Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer) which will actually try and solve the issues for you without you having to do anything. This is akin to auto-formatting your code.

#### Linting

The PHP command line binary has a built-in linter. You can invoke it with `php -l`. By default, the linter does not accept multiple files. You can use the following command to lint a folder of files:

```
find path/to/code/ -name "*.php" -print0 | xargs -0 -n1 -P8 php -l
```

It is recommended that you setup your editor to *lint on save*.

If you want to use a sublime build system, you can use the following setup:

```
{
  "cmd": ["php", "-l", "${file}"],
  "path": "/path/to/php",
  "selector": "source.php,source.html,source.phtml,source.inc"
}
```

An additional optional tool would be [PHP Mess Detector](https://github.com/phpmd/phpmd). A simple command to run on your code would be `phpmd path/to/code text codesize,unusedcode,naming`.

You can setup the `phpmd` build tool in sublime with the following rule:

```
{
  "cmd": ["phpmd", "${file}", "text", "codesize,unusedcode,naming"],
  "path": "${PATH}",
  "selector": "source.php"
}
```

#### Testing

Testing in PHP is typically done with [phpunit](https://phpunit.de/). Most frameworks have a mechanism for testing. So use the testing protocol outlined by the framework in use.

### Javascript (and JSON)

There are a variety of javascript linting and formatting tools. We will only cover one for each category.

#### Formatting

The first tool to setup is [js-beautify](https://github.com/beautify-web/js-beautify). This tool will format javascript and JSON files to make them easier to read and also easy to debug.

These are the rules for the formatter that Invoke uses:

```
{
  // exposed jsbeautifier options
  "indent_with_tabs": false,
  "preserve_newlines": true,
  "max_preserve_newlines": 4,
  "space_in_paren": false,
  "jslint_happy": false,
  "brace_style": "collapse",
  "keep_array_indentation": false,
  "keep_function_indentation": false,
  "eval_code": false,
  "unescape_strings": false,
  "break_chained_methods": false,
  "e4x": false,
  "wrap_line_length": 0
}
```

You should install the tool as a plugin in your editor and set the formatting to *apply on save*.

#### Linting

The linting tool we use is called [jshint](http://jshint.com/). It is run using the following rules:

```
{
  // Documentation: http://www.jshint.com/docs/options/
  "browser": true,
  "browserify": true,
  "node": true,
  "esnext": true,
  "esversion": 6,
  "jquery": true,
  "node": true,
  "globals": {
    "jQuery": true,
    "Vue": true
  },
  "strict": "implied",
  "undef": true,
  "unused": true
}
```

Just like the tools above, you should set this program up in your editor to *run on save*.

You can also add a `.jshintrc` file in your git project and the tool should look for that and use it.

#### Additional Tools

If you are not using an IDE, but are using a text editor like vim, atom, or sublime, then you should consider installing [ternjs](http://ternjs.net/).

Tern is a stand-alone code-analysis engine for JavaScript. It is intended to be used with a code editor plugin to enhance the editor's support for intelligent JavaScript editing. Features provided are:

* Autocompletion on variables and properties
* Function argument hints
* Querying the type of an expression
* Finding the definition of something
* Automatic refactoring

This can be incredibly helpful as you can essentially add IDE-like features to your projects. For example, if you want to use `Array.reduce`, but you forget the arguments, tern can show you the arguments, their types, and also a link to [documentation at MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference). It can also infer types for your own functions and methods that exist in your projects.

#### Testing

##### Backend

For unit testing, [node-tap](http://www.node-tap.org/) is extremely simple and easy to get started.

*TODO: other testing types.*

##### Frontend

Our projects that require frontend testing typically use [nightwatchjs](http://nightwatchjs.org/). Nightwatch also allows you to take advantage of the [new headless browsing feature](https://developers.google.com/web/updates/2017/04/headless-chrome) in Chrome >58.

You can also run tests in [phantomjs](http://phantomjs.org/), which doesn't boot up a browser at all. But you should be careful with this as it is not a replacement for a real browser test.

Any frontend tests *should pass in all major browser versions*.

### Editorconfig

[Editorconfig](http://editorconfig.org/) is a cross-platform editor plugin to adjust your editor to handle files in a specific way.

It will look for a file called `.editorconfig` in the root of your project and then apply those settings to the editor.

Here are a set of defaults to use in any `.editorconfig` files:

```
# EditorConfig: http://EditorConfig.org

# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf
trim_trailing_whitespace = true
insert_final_newline = true
charset = utf-8
indent_style = space
indent_size = 2

# PHP rules
[*.php]
indent_size = 4

[*.md]
trim_trailing_whitespace = false
```

These files should be committed to your git repository.