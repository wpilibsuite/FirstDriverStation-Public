# Logging

The DS logs into the wpilog format.

You can use the webpage to grab log files, otherwise they are located in the `~/.firstds` folder on unix platforms or `Public Documents/firstds` on Windows.

## Built in AdvantageScope

The webserver hosts a built in version of AdvantageScope. This can be accessed in 1 of 2 ways. First, you can navigate to `http://localhost:6768/ascope`. Second, in settings there is an `Open Log Viewer` button. The one in settings mostly works, but cannot be properly resized currently.

When you first load up AdvantageScope, it will automatically connect to the built in NT server in the DS. This means that you can live trace all the information that is sent between the frontend and backend on the DS.

You can also open log files directly by going to File -> Open Log
