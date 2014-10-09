DollarAt
========

This script demonstrates the use of the Bash `$@` variable.

It defines 4 functions : maybe, not, waitfor and assert.

The `$@` variable allows us to take arguments from the command line and use them
as a command without having the trouble to care about interpretation (variable
expansion, double quotes etc.).

Maybe function
--------------

The `maybe` function is used when you want to execute a command only if it
exists. If it does not, your program should be able to run without any trouble.

For example:

    maybe notify-send "This is a notification"

will call the `notify-send` command from `libnotify` if it exists. If it does
not, it’s not a problem.

Not function
--------------

The `not` function inverts the return code of the command it executes.

For example:

    not test -f /etc/foobar

will set the return code to 0.

WaitFor function
----------------

The `waitfor` function tries to run a function until a command works correctly
unless it takes too much time.

For example:

    waitfor 5 [ -f daemon.pid ]

will wait a maximum of 5 seconds for the `daemon.pid` file to appear. The
`waitfor` function returns 0 if the command returns 0, otherwise it returns 1.
You can therefore use it like this:

    waitfor 5 [ -f daemon.pid ] || exit 1

Assert function
---------------

The `assert` function imitates the instruction of the same name you can find in
many languages.

The first parameter is the error message to show in case of trouble. The rest of
the line is the command to execute.

For example:

    assert "Unable to find /etc/passwd" test -f /etc/passwd

Thanks to the `caller` function, the error message is displayed alongside the
line number which generated the error.

For example:

    assert "File not found" test -f /etc/foo/bar

will display on the standard error output:

    2014-10-09 08:10:40+0200 script [13632]: File not found (line=2, rc=1)
