This is a discord bot aimed at making it easy to schedule raids in a discord server. It is developed for LotRO but could work for any game if you edit the class names in the config file.

When the `!raid` command is called the bot will create an embed specifying the raid time in server time (New York), Los Angeles', London's and Sydney's time.
All time zones are configurable.
The bot will add class emojis to this embed for users to interact with.
When a user clicks on a class emoji it will update the embed listing the user's discord nickname and available classes.
Moreover it will add tools, pickaxe, check mark and cross mark emojis.
A user can sign up with all his (previously used) classes by clicking the green check mark and cancel his sign up by clicking the red cross mark.
One can reset the classes for the green check mark sign up via the `!roles` command.
The tools emoji can only be used by raid leaders to update the raid bosses, raid tier, raid time or roster settings.
The pickaxe emoji can be used by raid leaders to pick people for the raid from available sign ups.

The `!raid` command requires a name (of the raid) and a tier argument, which may get cumbersome.
Alternatively you can create a discord channels in your server, say `tier-2-raids` and `tier-3-raids` and use the nickname of the raid directly: `!anvil saturday 6pm`.
This command will look in the channel name for a number and uses that as tier argument and default to t1 if it cannot find any number.
It will expand the alias used to invoke the command as name for the raid (in this case Anvil of Winterstith).
See [list-of-raids](./source/list-of-raids.csv) for all nicknames that can be used to quickly schedule a raid.

![Screenshot](./screenshots/raid.png)

For a detailed explanation how to use the bot's commands please use `!help` once it is running.

If you would like to use the public bot please join our
[discord server](https://discord.gg/5YqSzuV)
to find the bot invite link.

If you would like to host your own instance, continue reading below.

------------------------------------

Prerequisites:\
python >= 3.6\
Check the requirements.txt file for required libraries.\
You can get all of them with: `python3 -m pip install -U -r requirements.txt`, assuming requirements.txt is in your current directory.

See details further below how to specify your configuration file and then simply run with `python3 main.py`

------------------------------------
**Instructions for Windows.**\
Python does not come pre-installed on Windows so you will need to install it yourself. Go to https://www.python.org/downloads/ and click the big yellow button to download Python 3.8. Simply run the downloaded file to install Python. Now to save yourself a world of pain, BEFORE you click the 'Install Now' button, make sure you check the box **'Add Python 3.8 to PATH'**. This will allow you to use the python command in your terminal (command prompt).

To check installation was successful open a terminal and type `python --version`, it will return (as of writing) Python 3.8.2, assuming you do not have Python 2 installed.
(Search for cmd and press enter to open a terminal.)
Download the latest release from https://github.com/Baviaan/lotro/releases/latest by clicking on the zip version and extract the folder on your computer.
In your terminal change directory to wherever you extracted the files.
(For example type `cd Desktop\lotro-3.7.0` if you downloaded version 3.7.0 and extracted the folder to your desktop.)
Now you can install the required Python libraries with `python -m pip install -U -r requirements.txt`.
Before you run the bot you will need to edit your configuration file.
See below for further instructions what to put in the configuration file.
Once you are done you can run the bot by typing `python main.py` in your terminal.

------------------------------------

You will need to make a copy of the 'example-config.json' file, name it 'config.json' and specify your configuration values.
(Windows might complain it doesn't know how to open json files, but simply select to open it with notepad and it'll work fine.)
See below for a guide how to create a discord bot.

Config file values:\
BOT_TOKEN: Your discord's bot token (this is not the client secret).\
CLASSES: The classes in your game. Note your discord server must have custom emojis named exactly the same. Emojis for LotRO are included, you can upload these to your discord server.\
LINEUP: A sequence of zeroes and ones indicating for each slot whether the class should be present, in the order as specified under CLASSES. This will **ABSOLUTELY BREAK THE UI** if you specify too many ones. Please contain yourself.\
CHANNELS: Any incoming applications will be posted to the APPLY channel, so this should be officer only.\
LEADER: Name of the discord role that will be allowed to update bosses and times for raid post.\
SERVER_TZ: The raid time in the header of the embed will be posted in this time zone. (Requires TZ database name.)\
TIMEZONES: The raid time will be displayed in these time zones additionally to server time. (Requires canonical TZ database names.)\
PREFIX: The prefix that will be used to start a command.\
LANGUAGE: The language of the bot. Currently only English "en" and French "fr" are supported.

See [es/messages.po](./source/locale/es/LC_MESSAGES/messages.po) if you wish to help translate to Spanish.
An example config file has been included for English and French.
**If language is not set to "en", the language binary file needs to be generated by running `msgfmt.py` using `messages.po` as input to create a file `messages.mo`.**

------------------------------------

See this link how to create a bot user on discord, obtain your bot token and invite the bot to your server:
https://discordpy.readthedocs.io/en/latest/discord.html#

Please ensure the bot has the correct permissions: 268724304.

(Manage roles, manage channels, send messages, manage messages, embed links, use external emoji, add reactions.)

------------------------------------

Any questions please ask in our discord server:
https://discord.gg/dGcBzPN

------------------------------------

## Command overview

### Configuration commands
| Command | Requirement | Example | Notes |
| ------- |:-----------:| ------- | ----- |
| **!prefix** \<prefix\> | Admin | !prefix ? | Changes the prefix for all commands. |
| **!leader** \<role_name \>| Admin | !leader Officer | Set to "Raid Leader" by default. Case sensitive. Raid leaders can edit raids posted by others. |
| **!servertime** \<timezone\> | Raid Leader | !servertime europe/paris | Set to US Eastern by default. See `!help servertime` for timezone format. This timezone is used in the header of a scheduled raid and is also the default timezone for interpretation of raid commands. |
| **!displaytimes** \[timezones...\] | Raid Leader | !displaytimes america/chicago america/los_angeles | Set to Sydney, London and Los Angeles by default. See `!help displaytimes` for timezone format. |

### Scheduling commands

| Command | Alias | Example | Notes |
| ------- |:-----:| ------- | ----- |
| **!raid** \<name\> \[tier\] \<time\> | **!instance**, **!r** | !raid anvil t3 friday 8pm | Schedules a custom raid. Name must be quoted if multiple words. Tier argument is optional. If omitted, it attempts to parse tier information from the channel name and defaults to t1 otherwise. If a timezone is omitted it will default to the user's preferred timezone (which is server time by default). |
| **!fastraid** \[tier\] \<time\> | **!rem**, **!ad**, etc | !rem  friday 8pm, !ad t3 friday 8pm | Faster way to schedule a raid. **Invoke this command with an alias.** Use `!help fastraid` for the full list of aliases.
| **!meetup** \<name\> \<time\> | **!meet**, **!m** | !meetup "kin house" friday 8pm | Similar to `!raid` but without a tier argument. |

### User specific commands
| Command | Example | Notes |
| ------- | ------- | ----- |
| **!roles** | !roles | Shows the class roles you have and lets you reset them. |
| **!timezone** \<timezone\> | !timezone europe/london | Set to server time by default. See `!help timezone` for timezone format. This timezone is used to interpret *your* raid commands. |

### Miscellaneous commands
| Command | Notes |
| ------- | ----- |
| **!privacy** | Displays information on data collection and retention. |
| **!welcome** | Resends the welcome message. |
| **!about** | Shows some basic information about the bot. |
| **!help** [command] | The command argument is optional. If omitted it will show the list of all commands. If provided it will show detailed help for that command. |
