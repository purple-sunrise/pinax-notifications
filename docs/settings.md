# Settings

The following allows you to specify the behavior of `pinax-notifications` in
your project. Please be aware of the native Django settings which can affect
the behavior of `pinax-notification`.


## PINAX_NOTIFICATIONS_BACKENDS

Formerly, this setting was `NOTIFICATION_BACKENDS`.

Defaults to:

    [
        ("email", "pinax.notifications.backends.email.EmailBackend"),
    ]


## PINAX_USE_SSL

_This is a proposed common setting across the Pinax ecosystem. It currently may
not be consistant across all apps._

Formerly, this setting was `DEFAULT_HTTP_PROTOCOL` and defaulted to `http`.

It now defaults to `False`.

This is used to specify the beginning of URLs in the default `email_body.txt`
file. A common use-case for overriding this default might be `https` for use on
more secure projects.


## PINAX_NOTIFICATIONS_LANGUAGE_MODEL

Formerly, this setting was `NOTIFICATION_LANGUAGE_MODULE`

There is not set default for this setting. It allows users to specify their own
notification language.

Example model in a `languages` app::

    from django.conf import settings

    class Language(models.Model):
    
        user = models.ForeignKey(User)
        language = models.CharField(max_length=10, choices=settings.LANGUAGES)


Setting this value in `settings.py`::

    PINAX_NOTIFICATIONS_LANGUAGE_MODEL = "languages.Language"


DEFAULT_FROM_EMAIL
------------------

Defaults to `webmaster@localhost` and is a [standard Django setting](https://docs.djangoproject.com/en/1.7/ref/settings/#default-from-email).

Default e-mail address to use for various automated correspondence from
`pinax.notifications.backends.email`.


## LANGUAGES

Defaults to a tuple of all available languages and is a
[standard Django setting](https://docs.djangoproject.com/en/1.7/ref/settings/#languages).

The default for this is specifically used for things like the Django admin.
However, if you need to specify a subset of languages for your site's front end
you can use this setting to override the default. In which case this is the
definated pattern of usage::

    gettext = lambda s: s

    LANGUAGES = (
        ("en", gettext("English")),
        ("fr", gettext("French")),
    )


## PINAX_NOTIFICATIONS_QUEUE_ALL

Formerly, this setting was `NOTIFICATION_QUEUE_ALL`.

It defaults to `False`.

By default, calling `notification.send` will send the notification immediately,
however, if you set this setting to True, then the default behavior of the
`send` method will be to queue messages in the database for sending via the
`emit_notices` command.


## PINAX_NOTIFICATIONS_LOCK_WAIT_TIMEOUT

Formerly, this setting was `NOTIFICATION_LOCK_WAIT_TIMEOUT`.

It defaults to `-1`.

It defines how long to wait for the lock to become available. Default of -1
means to never wait for the lock to become available. This only applies when
using crontab setup to execute the `emit_notices` management command to send
queued messages rather than sending immediately.


## PINAX_NOTIFICATIONS_SETTING_MODEL

This defaults to `notifications.NoticeSetting`.

It exists so that you can supply your own subclass of
`pinax.notifications.models.NoticeSettingBase` in case you needed to scope
settings by some group or team in your site. For example, if you were using
`pinax-teams` and wanted people to be able to set/control notifications
differently for each team of which they were a member.

See [scoping your notifications](scoping.md) for more details on how this
works.
