Code Formatting & Quality Rules
===============================

### Why?

In order to help reduce cognitive overhead when joining a new project started by another developer, we apply coding standards and formatting to all our code.

If everyone on the team can get on board with a particular style, we can make it easier to pass projects around and contribute to each others work.

## Tools

The standards outlined here are broken down into 2 groups: formatting and linting.

### PHP

PHP has a community suggestions around different coding practices and standard. These are called *PHP Standard Recommendations* or *PSRs*.

#### Formating

Formatting for PHP code should be in the format described in [PSR-2](http://www.php-fig.org/psr/psr-2/). You should install and setup [PHP Code Sniffer](https://github.com/squizlabs/PHP_CodeSniffer) in your editor of choice and set the tool *to run on save*. You should make sure that the `--standard=PSR2` flag is always used when the command is run.

If you want to run the program from the command line, use the following command: `phpcs -n --standard=PSR2 --extensions=php --colors path/to/code`.

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