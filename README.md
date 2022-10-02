# telegram.bot   <img src="resources/telegram.bot.logo.png" alt="telegram.bot" title="telegram.bot" align="right" style="filter: brightness(var(--value, 200%)" data-type="%" height="180"/>
[![GitHub license](https://img.shields.io/github/license/beep-projects/telegram.bot)](https://github.com/beep-projects/telegram.bot/blob/main/LICENSE) [![shellcheck](https://github.com/beep-projects/telegram.bot/actions/workflows/shellcheck.yml/badge.svg)](https://github.com/beep-projects/telegram.bot/actions/workflows/shellcheck.yml) [![GitHub forks](https://img.shields.io/github/forks/beep-projects/telegram.bot)](https://github.com/beep-projects/telegram.bot/network) [![GitHub stars](https://img.shields.io/github/stars/beep-projects/telegram.bot)](https://github.com/beep-projects/telegram.bot/stargazers) ![GitHub repo size](https://img.shields.io/github/repo-size/beep-projects/telegram.bot)  

[![Scc Count Badge](https://sloc.xyz/github/beep-projects/telegram.bot/?category=code)](https://github.com/beep-projects/telegram.bot/) [![Scc Count Badge](https://sloc.xyz/github/beep-projects/telegram.bot/?category=blanks)](https://github.com/beep-projects/telegram.bot/) [![Scc Count Badge](https://sloc.xyz/github/beep-projects/telegram.bot/?category=lines)](https://github.com/beep-projects/telegram.bot/) [![Scc Count Badge](https://sloc.xyz/github/beep-projects/telegram.bot/?category=comments)](https://github.com/beep-projects/telegram.bot/) [![Scc Count Badge](https://sloc.xyz/github/beep-projects/telegram.bot/?category=cocomo)](https://github.com/beep-projects/telegram.bot/) ![visitors](https://visitor-badge.glitch.me/badge?page_id=beep-projects.telegram.bot)



`telegram.bot` is a bash script to send Telegram messages on [Debian](https://www.debian.org/)-based distributions.

Goal is to have the installation foot print as low as possible, but dependencies are [curl](https://curl.se/) and [jq](https://stedolan.github.io/jq/).

This script is based on https://github.com/NicolasBernaerts/debian-scripts/tree/master/telegram 



# Contents

- [Usage](#usage)
- [Install/Update](#installupdate)
- [Uninstall](#uninstall)
- [Commands](#commands)
  - [help](#help)
  - [test bot token](#test-bot-token)



## Usage

[(Back to Contents)](#contents)

For using `telegram.bot`, you need a **Telegram** account and app. See [telegram.org](https://telegram.org/) on how to set this up.  Once you have Telegram installed, you need to create a **bot**. This can be done by talking to **[@BotFather](https://core.telegram.org/bots#6-botfather)** in your Telegram app. [Follow this guide](https://core.telegram.org/bots#6-botfather), or google for it.

The **API token** is a string like `110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw` that is required to authorize your requests for communication via the Bot API.  Once you have obtained an API token to authorize your bot, you can start using `telegram.bot`.

First thing you should do is to test if your API token is valid. The test will finish silently, if the API token is valid, and it will return the Telegram server's reply, if the API token is invalid, e.g.

```bash
beep@projects:~$ telegram.bot --test_token 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw
{"ok":false,"error_code":404,"description":"Not Found"}
beep@projects:~$ 
```

Once you have an API token for your bot, you need a **chat id** that the bot should talk to. The chat id is not easy to find, but `telegram.bot` will help you.

You have two options:

- You either make the bot to chat with you directly, then you need your personal chat id.
- Or you make the bot to chat with a group, then you have to add the bot first to that group. 

<sub>(One note about bots in groups: By default, bots can only read **commands** which are posted in groups and not all messages. A command is a single words (`[a-z_0-9]{1,32}`) with a prefixed `/`, e.g. `/help`. If you want that your bot can receive all messages from a group, you have to promote it to be a group admin. )</sub>

What ever your choice is, send a message to the group, or to your bot and call the following command. This will give you a list of chat ids, from which the bot got updates.

```bash
beep@projects:~$ telegram.bot --get_chatid --bottoken 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw
[
  {
    "chatid": -1337818101234,
    "name": "condocam.ai"
  },
  {
    "chatid": -575749100,
    "name": "SystaPi"
  },
  {
    "chatid": 8339234211,
    "name": "Beep"
  }
]
```

With that information, you are ready to go!

```bash
beep@projects:~$ telegram.bot --chatid 8339234211 --bottoken 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw --text "Hello World"
```

Messages are build up by four elements (`--document` || `--photo`), `--icon`,`--title` and `--text`  which can be used stand alone, or in combination.

<img src="/home/freak/git/telegram.bot/resources/usage_example.png" alt="usage_example" align="left" style="zoom:33%;" /> 

```bash
beep@projects:~/git/telegram.bot$ ./telegram.bot --bottoken ***REMOVED*** --chatid ***REMOVED*** --photo resources/telegram.bot.logo.png --success --title "Welcome to telegram\.bot" --text "The *text area*\n_can_ have ~one~ __multiple__ lines\nand Emojis \U1f44d\!\nBut don't forget to escape the reserved characters like \., \*, \_, \-, \[, \], etc\. if you want to use them in your messages\.\nVisit [beep\-projects](https://github.com/beep\-projects/) for more fun projects\."
```



## Install/Update

[(Back to Contents)](#contents)

The script has build in functionality to copy itself to `/usr/local/bin`, which should be in the path of most Linux distributions. It also resolves its dependencies on `curl` and `jq` via apt, which binds it to Debian-based distributions. For updating the script, you can run the same routine as for installation, or just copy the new version of the script on your own to `/usr/local/bin`.

```bash
wget https://github.com/beep-projects/telegram.bot/telegram.bot
chmod 755 telegram.bot
sudo ./telegram.bot --install
```



## Uninstall

[(Back to Contents)](#contents)

To uninstall the script, you just have to remove it from `/usr/local/bin`. 
You have to decide if you also want to remove `curl` and `jq` and remove these packages on your own.

```bash
sudo rm /usr/local/bin/telegram.bot
sudo apt purge jq curl
```



## Commands

[(Back to Contents)](#contents)

The script makes use of the official [Telegram Bot API](https://core.telegram.org/bots/api). Please check the API for commands that you would like to get added to `telegram.bot` and start a [new discussion](https://github.com/beep-projects/telegram.bot/discussions/new) for it.



### help

[(Back to Commands)](#commands)

Displays the help text on command usage

```bash
beep@projects:~$ telegram.bot --help
  telegram.bot: Tool to send a message to a Telegram User or Channel.
  Message is sent from a Telegram Bot and can contain icon, text, image and/or document.
  Main parameters are :
    -h/-?/--help           display this help and exit
    --install              install dependencies, copy telegram.bot to /usr/local/bin and exit (run as root)
    --get_chatid  <timeout> calls getUpdates on the bot for <timeout> seconds and returns the chart id for the first found message
    -test/--test_token     Test if the provided --bottoken is valid
    -del/--delete_commands Delete all commands for the bot identified by --bottoken
    -set/--set_commands "command1=description1" "command2=description2" ... Set the commands for this bot
    -t/--text <text>       Text of the message (use - for piped text)
    -f/--file <file>       File holding the text of the message
    -p/--photo <file>      Image to display
    -d/--document <file>   Document to transfer
  Options are :
    --title <title>        Title of the message
    --html                 Use HTML mode for text content (markdown by default)
    --disable_preview      Don't create previews for links, image and/or document
    --silent               Send message in silent mode (no user notification on the client)
    -q/--quiet             Don't print message to stdout
    -v/--verbose           explain what is being done
    --print_logo           print the telegram.bot logo
    -cid/--chatid <chat-id>  Recipient User or Channel ID
    -bt|-token|--bottoken <bot-token>  API token of your Telegram bot
  Optional icons are :
    --success              Add a success icon
    --warning              Add a warning icon
    --error                Add an error icon
    --question             Add a question mark icon
    --icon <code>          Add an icon by UTF code (ex 1F355)
  Here is an example of piped text :
    echo 'text to be displayed' | telegram --success --text -

```



### test API token

[(Back to Commands)](#commands) [(Bot API getMe)](https://core.telegram.org/bots/api#getme)

Calls [getMe](https://core.telegram.org/bots/api#getme) funtion of the Bot API and checks the returned JSON. 

If the result is ok, nothing is returned

If the result is not ok, the reply from the server is returned

```bash
beep@projects:~$ telegram.bot --test_token 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw
{"ok":false,"error_code":404,"description":"Not Found"}
beep@projects:~$ 
```



### get chatid

[(Back to Commands)](#commands)



```bash
EXAMPLE
```



### set commands

[(Back to Commands)](#commands) [(Bot API sendMessage)](https://core.telegram.org/bots/api#sendmessage)



```bash
telegram --bottoken 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw --chatid 8339234211 --set_commands "help=show commands list" "ping=return pong" "reboot=reboot bot server" "shutdown=shut down bot server" "restartme=restart motioneye.service" "status=get system status" "snapshot=get snapshots from all cameras" "uptime=call uptime" "df=call df -h" "mdon=enable motion detection" "mdoff=disable motion detection" "setcommands=update commands at @BotFather"
```



### delete commands

[(Back to Commands)](#commands)



```bash
EXAMPLE
```



### send text

[(Back to Commands)](#commands) [(Bot API sendMessage)](https://core.telegram.org/bots/api#sendmessage)

Use this method to send text messages.

```bash
telegram.bot --bottoken 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw --chatid 8339234211 --text "Hello World"
```

This command requires the following flags `-bt | -token | --bottoken <api token>`, `-cid|--chatid <chat id>`, `-t|--text <string>` 

#### Optional flags

`--html` Default formatting of text or photo caption is [MarkdownV2 style](https://core.telegram.org/bots/api#markdownv2-style) but you can change it to [HTML style](https://core.telegram.org/bots/api#html-style) by setting the `--html` flag

`--disable_preview` Disables link previews for links in this message

`--silent` Sends the message [silently](https://telegram.org/blog/channels-2-0#silent-messages). Users will receive a notification with no sound.

`--protected`  Protects the contents of the sent message from forwarding and saving

`-q | --quiet` suppresses error messages and takes care that `telegram.bot` is not returning any status information.

`-v|--verbose` tells the script to print out additional information on what is going on

`-i | --icon <code>` adds an icon to the sent message. For easier use, `--success` (:white_check_mark:, 2705), `--warning` (:warning:, 26A0), `--error` (:rotating_light:, 1F6A8), and `--question` (:question:, 2753)  are, predefined icons. You can find other codes to use in the [Emoji List](https://unicode.org/emoji/charts/full-emoji-list.html).



`-get_offset | --get_update_offset`

`-p | --picture | --photo <path>`

`-d | --document <path>`

`--title <string>`



#### incompatible flags

`-h | -? | --help`

`--file`

`-test | --test_token` 

`--get_chatid`

`--print_logo`

`--install`

`--get_updates`

`-del | --delete_commands`

`-set | --set_commands "<cmd1>=<descr1>"` 

#### ignored flags

`--timeout`

`--offset`

### pipe text to send

[(Back to Commands)](#commands) [(send text)](#send-text)

A special case to send text is piping (`|`) the text to `telegram.bot`, e.g. the output of a command. This can be done by using `-` as single argument to `--text -`.

```bash
echo 'Hello World' | telegram.bot --bottoken 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw --chatid 8339234211 --text -
```

You can use all other flags that you can also use with [--text](#send-text).

### send a photo

[(Back to Commands)](#commands) [(Bot API sendPhoto)](https://core.telegram.org/bots/api#sendphoto)	

Use this method to send photos.

```bash
EXAMPLE
```



### send text from a file

[(Back to Commands)](#commands) 

This function reads the content from a text file and sends it

```bash
EXAMPLE
```

Text passed via `--text` will be overwritten with the content of the text file



### send a document

[(Back to Commands)](#commands) [(Bot API sendDocument)](https://core.telegram.org/bots/api#senddocument)

Use this method to send general files. On success, the sent [Message](https://core.telegram.org/bots/api#message) is returned. Bots can currently send files of any type of up to 50 MB in size, this limit may be changed in the future.

```bash
EXAMPLE
```



### flags compatibility matrix

r = required

o = optional

| arguments:arrow_right:<br />:arrow_down: commands            | `--bottoken` | `--chatid` | `--disable_preview` | `--html` | `--icon` `--error` `--success` `--question` `--warning` | `--offset` | `--quiet` | `--silent` | `--protected` | `--text` | `--timeout` | `--title` | `--verbose` |
| ------------------------------------------------------------ | :----------: | :--------: | :-----------------: | :------: | :-----------------------------------------------------: | :--------: | :-------: | :--------: | :-----------: | :------: | :---------: | :-------: | :---------: |
| **`--delete_commands`**                                      |      r       |            |                     |          |                                                         |            |     o     |            |               |          |             |           |      o      |
| **`--document`**                                             |      r       |     r      |                     |    o     |                            o                            |            |     o     |     o      |       o       |    o     |             |     o     |      o      |
| **`--install`**                                              |              |            |                     |          |                                                         |            |     o     |            |               |          |             |           |      o      |
| **`--file`**                                                 |      r       |     r      |          o          |    o     |                            o                            |            |     o     |     o      |       o       |    '-    |             |     o     |      o      |
| **`--get_chatid`**                                           |      r       |            |                     |          |                                                         |     o      |     o     |            |               |          |      o      |           |      o      |
| **`--get_update_offset`**                                    |      r       |            |                     |          |                                                         |            |     o     |            |               |          |             |           |      o      |
| **`--get_updates`**                                          |      r       |            |                     |          |                                                         |     o      |     o     |            |               |          |             |           |      o      |
| **`--help`**                                                 |              |            |                     |          |                                                         |            |     o     |            |               |          |             |           |      o      |
| **`--print_logo`**                                           |              |            |                     |          |                                                         |            |     o     |            |               |          |             |           |      o      |
| **`--photo`**                                                |      r       |     r      |                     |    o     |                            o                            |            |     o     |     o      |       o       |    o     |             |     o     |      o      |
| **`--set_commands`**                                         |      r       |            |                     |          |                                                         |            |     o     |            |               |          |             |           |      o      |
| **`--icon`** **`--error`** **`--success`** **`--question`** **`--warning`** |      r       |     r      |                     |    o     |                                                         |            |     o     |     o      |       o       |    o     |             |     o     |      o      |
| **`--test_token`**                                           |      r       |            |                     |          |                                                         |            |     o     |            |               |          |             |           |      o      |
| **`--text`**                                                 |      r       |     r      |          o          |    o     |                            o                            |            |     o     |     o      |       o       |          |             |     o     |      o      |
| **`--title`**                                                |      r       |     r      |                     |    o     |                            o                            |            |     o     |     o      |       o       |    o     |             |           |      o      |

