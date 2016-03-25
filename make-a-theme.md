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

How else can I reference a timecard?
---

Timecards can also be referenced like this:
- `username/repo:path/to/theme.ejs`
- `http://example.com/path/to/theme.ejs`

Or, send us a pull request in <https://github.com/waltz-app/themes> and then
reference like this:
- `theme`

Where can I find "x" in the timecard?
---

#### General
- `timecard.name`: The name of the project or entity work is being done for.
- `timecard.tagline`: A secondary description of the project, usually pertaining
  to the work done / more info about the business.
- `timecard.address`: The location of the business; this is represented as an array, with each item being a subsequent line in the address.
- `timecard.primaryColor`: The primary color associated with the branding of the invoice.
- `timecard.secondaryColor`: The secondary color associated with the branding of the invoice.
- `timecard.reportFormat`: The default report the timecard is set to be rendered
  to when run with `waltz render`.
- `timecard.card`: A list of times that the user has worked for. This is the
  "meat" of the whole data structure.
- `timecard.taxPercent`: How much tax should be applied to the price, specified as a float from `0` to `1` (ie, `0.15` would be 15%)

#### Client Rates
**NOTE: Both of these properties could be defined, and if there is a choice,
default to using the hourly rate over the total rate.**
- `timecard.hourlyRate`: The rate the client charges per hour for work completed. This property can be undefined.
- `timecard.totalRate`: The total cost the client has decided to charge for work completed, without tax.

### Totals
- `totalCost`: The total amount that the client owes.
- `totalTime`: The total time worked by the client.
- `paidTime`: The already paid time worked by the client.
- `paidCost`: The total previously-paid money to the client. This is only really relevant when the client is billed hourly, so this is `null` if `timecard.hourlyRate` isn't set.
