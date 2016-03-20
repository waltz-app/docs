Create a timecard theme
===

Timecard themes are just a specially formatted ejs file. [More on EJS here](http://ejs.co/). Within the file, a
couple globals are exposed:

- `timecard`: The timecard to render.
- `args`: Arguments passed to the renderer.
  Serverside, `timecard.args` provides a base for users to provide default arguments and this value will always equal that.
  However, if rendering clientside, this object comes from `timecard.args` and a list of command-line arguments merged together.
  For example, if the timecard's args are `{"foo": "baz", "joe": "sue"}` and is
  rendered with `waltz report --foo bar --abc xyz`, `args` will be
  `{"foo": "bar", "abc": "xyz", "joe": "sue"}`

- `totalTime`: The total amount of unpaid time, in seconds.
- `totalCost`: If the user has specifified an hourly rate, the total price that is
owed by the client. (This only takes into account unpaid times.)

An guided example
---

Let's create a timecard theme! Lets start by creating a new file called
`theme.ejs` in your project's directory. Within the file, lets give the theme
some content:
```ejs
We've worked for <%= totalTime %>, and earned <%= totalCost %>.
```

Next, open `.timecard.json` and change the `reportFormat` key to equal
"./theme.ejs" to tell Waltz which theme to use.  (This file should be referenced relative to the location of the `.timecard.json` you are editing right now.)

To render the timecard, run `waltz render`. In your current directory, open the
new file `report.html`, and there's the timecard.

**NOTE: Before pushing the timecard to Github, be sure to change the
`reportFormat` key to a different format (see below). The waltz server app won't
be able to reference the theme properly and wierd things can happen.**

Some notes
---

Timecards can also be referenced like this:
- `username/repo:path/to/theme.ejs`
- `http://example.com/path/to/theme.ejs`

Or, send us a pull request in <https://github.com/waltz-app/themes> and then
reference like this:
- `theme`
