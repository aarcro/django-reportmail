Let the hacking begin
=====================

Basic Usage
-----------

It's really easy and simple to use django-reportmail.
Only thing you should do is decorating the `handle` method of Django's management command
by `reportmail.command.apply_reporter`:

.. code-block:: python

    import csv
    from django.core.management.base import BaseCommand

    from reportmail.command import apply_reporter

    class Command(BaseCommand):
        @apply_reporter("Title")
        def handle(reporter, filepath, *args, **options):
            for i, l in enumerate(csv.DictReader(open(filepath))):
                reporter.append('Line {}: processed {l}'.format(i+1, l))


Then the `handle` method will take a `reporter` object as the first argument.
This reporter object is an interface to store messages which you want to notify to administrators.

Reporter
--------

The reporter object provide two method `reporter.append()` and `reporter.extend()`.
If you want to store a line of message, use `reporter.append()`. This method will take a string
object and store.
And if you want to store multiple lines of message, use `reporter.extend()`. This method will
will take a list object of strings and store.

Something you should write is storing messages to reporter as same as logging.
You will never write another messy codes.

The report
----------

When the command ends, administrators will get report mail.
By default, the mail will be like this::

    Subject:
        Title
    Body:
        Report of someapp.management.commands.some_of_your_command
        args: path/to/somecsv.csv
        options:

        result:
        Line1: processed {'somefield': 'somevalue0'}
        Line2: processed {'somefield': 'somevalue1'}
        Line3: processed {'somefield': 'somevalue2'}
        Line4: processed {'somefield': 'somevalue3'}

Notice that the subject of the mail is same value of the argument for `apply_reporter` decorator.
Actually the first argument of it will used as subject of the mail.

The head of the body is the condition of management command.
The first line is the name of command (module path of the command).
The second line is command arguments. And the third line is command options.

After them, It is showing the result of command, which is actually strings
you stored by calling `reporter.append()` or `reporter.extend()`.

If some unexpected error occurred while processing the command,
`apply_reporter` will catch the error and report it and it's traceback.

Always in motion is the future...
---------------------------------

You learned basic usage of django-reportmail.
But sometimes it's not enough to address some sort of customising.

On the next, you can learn advanced topics like changing mail templates, or changing way to report.
Let's continue :doc:`advanced`.