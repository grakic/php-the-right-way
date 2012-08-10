---
layout: page
title: Error Reporting in PHP
---

# Error Reporting in PHP

PHP can report errors while running your code, like the syntax error (also known as parse error) or the error of
accessing previously undefined variable or calling the undefined function. For example:

> PHP Fatal error:  Call to undefined function foo() in test.php on line 13

Reported errors are described with different levels, from a notice to the fatal error like above. A separate level
exists to add a warning when using deprecated features in the code that may be removed from the language in the future.

When testing your application, error reporting is useful in finding problem spots. You should not ignore any reported
error no matter how small it may look.

Reported error will include a type, a short summary and a file and line location of the error in the code. Parse errors
may report on a line following the real error location as PHP do not know what code you want to write and will stop
executing only after reading a non-recognizable construct.

There are a few PHP configuration directives for error reporting that can be set in `php.ini` (or what else your PHP
environment uses for placing PHP configuration).

It is possible to enable and disable error reporting for specific levels by setting the `error_reporting` configuration
directive, or dynamically using the `error_reporting()` function. Configuration directive `log_errors` can enable error
logging and `error_log` tells if errors should be logged into a file, to the web server log or the system logging
facility. Another directive `display_errors` configures if reported errors will be displayed in the output (e.g. in
the web page content).

It is recommended to set `error_reporting = E_ALL`.

For PHP 5.3 and earlier not all error types are included in `E_ALL`. You can set `error_reporting = -1` to workaround
this issue.

Configuration directive `display_errors` __must be Off__ in the production environment as it can expose details about
your application to the attacker, making it __easier to exploit any security issues__ in it. In cases where foreign
input is included in the error context, having this directive enabled may enable Cross-Site Scripting attacks.

In the development, with `display_errors` disabled you will need to constantly monitor the log for any errors reported
in your code. This does not have to be hard, a simple terminal window tailing the log file will be just fine. If that is
not right for you or if you are developing only simple web or CLI applications, you __may set `display_errors = On` only
for your local development environment__ to help you see and fix reported errors more easily.

In some cases displaying errors can break your output making reported errors hard to read. This is why some developers
may prefer to look at log for errors and disable `display_errors` in the development too. Imagine writing code to
respond on AJAX requests with a structured output format like JSON, or working with binary formats while outputting
dynamically generated images. If reported errors are displayed at random places in the output it will break the output
format, making it invalid and the error text may be hard to read. All this will make development and testing very
frustrating.

It is possible to override default error reporting with a custom error handler using the `set_error_handler()` function.
Many frameworks do have their own error handlers enabled by default with different settings for the developing and the
production environment. Some PHP extensions may change the default error reporting handler to provide more features.

Custom error handler can display fallback content or a custom error text without sensitive internal details to the user.
There are errors that may be triggered before custom error handler is set in code so you still need to configure
`display_errors` and other directives as recommended.

There are browser extensions and third party error reporting handlers to log reported errors from a web application
directly to the web browser console. This may be used in the development if one finds separate error logging too
inconvenient to setup and monitor.

Custom runtime errors may be triggered in the code using `trigger_error()` function, but using exceptions is usually a
better choice.

For more information on these settings, see the PHP manual:

* [Read about error handling and logging][errorhandling]
* [Read about configuration directives on error reporting][errorhandling_configuration]

[errorhandling]: http://www.php.net/manual/en/book.errorfunc.php
[errorhandling_configuration]: http://www.php.net/manual/errorfunc.configuration.php

