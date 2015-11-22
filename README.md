# Exhaust Emitters - CLI Logging

Comannd Line Interface (CLI) logging is one type of exhaust emitter, a component of the [Exhaust Proof-of-Concept](https://github.com/ThatLarryPearson/exhaust-PoC).

CLI logging takes advantage of Ubuntu `/etc/profile` processing and applies to login accounts configured to use `/bin/bash`.  Account shell configuration is set in `/etc/passwd`, [the password file](http://manpages.ubuntu.com/manpages/hardy/man5/passwd.5.html).

An [environment variable](https://help.ubuntu.com/community/EnvironmentVariables) customization script `CmdLogging.sh` is added to `/etc/profile.d` to get login shells to log shell commands.

## Installation

Use `git` to fetch the necessary file.

```
sudo apt-get install git-core
git clone https://github.com/ThatLarryPearson/exhaust-emitter-cli-logging.git
cd exhaust-emitter-cli-logging
sudo cp etc/profile.d/CmdLogging.sh /etc/profile.d/CmdLogging.sh
sudo chown root:root /etc/profile.d/CmdLogging.sh
sudo chmod 0755 /etc/profile.d/CmdLogging.sh
ls -l /etc/profile.d/CmdLogging.sh
```

## Testing

To test, do the following steps as root:

```
sudo su - root
. /etc/profile.d/CmdLogging.sh
wc -l /etc/profile.d/CmdLogging.sh
grep root:root /var/log/messages
```

## Customizing

The "logger" command is used to inject log messages into syslog.  We invoke the logger command whenever there is a shell command.
```
# shell startup is logged this way
logger -p "${Facility}.${Priority}" "shell[$$] $(id -un):$(id -gn) $(tty) PATH=${PATH}"

# commands are logged this way
logger -p "${Facility}.${Severity}" "shell[$$] $(id -un):$(id -gn) $(tty): ${command}"
```

I got the Facility and Severity string representations by running the "string" command against the logger binary.  There doesn't seem to be any official documentation describing facility/severity values that work with logger.
```
strings /usr/bin/logger
```

### Logger Command Facility Types
- auth
- authpriv
- cron
- daemon
- kern
- mail
- mark
- news
- security
- syslog
- user
- uucp
- local0
- local1
- local2
- local3
- local4
- local5
- local6
- local7

### Logger Command Severity Types
- alert
- crit
- debug
- emerg
- error
- info
- none
- notice
- panic
- warn
- warning

## Licensing

The MIT License (MIT)

Copyright &copy; 2015 Lawrence Bennett Pearson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


