
TAFE Queensland, has a website to access the [class timetables](https://timetables.tafeqld.edu.au/group). However, it only gives output in the form of text. But we want to put it into [Google Calendar](https://calendar.google.com). Data entry is bad, so let's automate it using `python`!

This project has a companion blog post on my blog [here](https://simonmcmahon.com/blog/tafe-timetable-python/).

The website text output looks like this:

![website screenshot](/images/tafe-timetable-python/timetable.png)

And when we copy the text from the page, we get output (into the `xed` text editor - the default on `Linux Mint`) like this:

![copied text](/images/tafe-timetable-python/copied-text-timetable.png)

So the format is:

```text
DATE1
\n
CLASS1
    TIMES
    'Room'
    ROOM LOCATION
    'Unit(s)'
    UNIT TEXT 1
    UNIT TEXT 2
\n
CLASS2
...

DATE2
...

```
## Parsing the text

So we can seperate out the key variables with lines as something like:

* Date: Does it match the `WEEKDAY, MONTH MONTHDAY, YEAR` format?
* TIMES: Contains `"AM"` or `"PM"`.
* Class N: `TIMES` line to a new line (`\n`).
* ROOM LOCATION: Line following `"Room"`.
* UNITS: Line text after `"Units(s)"` until new line (`\n`).

So firstly, we split text into an array of strings for each DATE. Then split each date string into a CLASS string. 
Then we build up each `CLASS` object with date, time, room and units.

## Making it an ics file

Then we need some calendar parsing libraries. From this [tutorial](https://www.tutorialsbuddy.com/create-ics-calendar-file-in-python), the [icalendar](https://pypi.org/project/icalendar/) seems well support with the features we need.

## The Code

All the juicy bits are contained in `python-ics.py`. Run this in the terminal by doing:

```shell
# Get this repo
git clone https://github.com/simon-mcmahon/tafeqld-timetable-ics-parsing
# Install the dependencies
pip install icalendar
# Go into folder. Make executable. Run :D
cd ./tafeqld-timetable-ics-parsing
chmod +x python-ics.py
./python-ics.py
```

## Importing to Google Calendar

From google support [here](https://support.google.com/calendar/answer/37118?hl=en&co=GENIE.Platform%3DDesktop#zippy=):

![import google calendar](/images/tafe-timetable-python/import-gcal.png)

Here are the results of the output if you decide to replicate this for yourself.

The terminal output:

![terminal output](/images/tafe-timetable-python/terminal-output.png)

The Google Calendar view:
![calendar view](/images/tafe-timetable-python/calendar-view.png)

Another Google Calendar event view (you can see the correct parsing of the dates into the event):
![date-parsing-verify](/images/tafe-timetable-python/date-parsing-verify.png)

