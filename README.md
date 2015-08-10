critbot
=======

Sending critical errors from python to syslog, slack, email, {your_plugin}.

Install:

    pip install critbot

Add to "config.py" file:

    import critbot.plugins.syslog
    import critbot.plugins.slack
    import critbot.plugins.email
    from critbot import crit_defaults

    crit_defaults.subject = 'MyService host:port CRIT'

    crit_defaults.plugins = [
        critbot.plugins.syslog.plugin(),
        critbot.plugins.slack.plugin(
            token='Get it from https://my.slack.com/services/new/bot',
            channel='#general', # '@private' or '#channel'
            users='', # '@user1 @user2 @userN'
        ),
        critbot.plugins.email.plugin(
            to='Name1 <user1@example.com>, Name2 <user2@example.com>',
            user='critbot@example.com', # Add more config if not GMail.
            password='pa$$word',
        ),
    ]

Check other config options and their defaults, e.g. "seconds_per_notification=60" and "spam=False":
* https://github.com/denis-ryzhkov/critbot/blob/master/critbot/core.py#L23 - "crit_defaults"
* https://github.com/denis-ryzhkov/critbot/blob/master/critbot/core.py#L38 - "crit"
* https://github.com/denis-ryzhkov/critbot/blob/master/critbot/plugins/syslog.py#L17
* https://github.com/denis-ryzhkov/critbot/blob/master/critbot/plugins/slack.py#L14
* https://github.com/denis-ryzhkov/critbot/blob/master/critbot/plugins/email.py#L14

Use "crit" in other files of your project:

    from critbot import crit

    try:
        1/0
    except Exception:
        crit()
        # More processing if needed.

    try:
        1/0
    except Exception:
        crit(also='test2')

    if True:
        crit('test3')

Please fork https://github.com/denis-ryzhkov/critbot  
and create pull requests with new plugins inside.

critbot version 0.1.4  
Copyright (C) 2015 by Denis Ryzhkov <denisr@denisr.com>  
MIT License, see http://opensource.org/licenses/MIT
