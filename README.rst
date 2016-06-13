aiosmtplib
==========

Introduction
------------

Aiosmtplib is an implementation of the python stdlib smtplib using asyncio, for
use in asynchronous applications.

Basic usage::

    import asyncio
    import aiosmtplib

    @asyncio.coroutine
    def send_a_message():
        smtp = aiosmtplib.SMTP(hostname='localhost', port=25)
        yield from smtp.connect()
        # Optionnally, login:
        yield from smtp.login('USERNAME', 'PASSWORD')
        yield from smtp.sendmail(
            'Root <root@localhost>',
            ['somebody@localhost'],
            "Hello World",
        )
        yield from smtp.close()

    asyncio.get_event_loop().run_until_complete(send_a_message())


Or, with Python 3.5:
    import asyncio
    import aiosmtplib

    async def send_a_message():
        async with aiosmtplib.SMTP(hostname='localhost', port=25) as smtp:
            # Optionnally, login:
            await smtp.login('USERNAME', 'PASSWORD')
            await smtp.sendmail(
                'Root <root@localhost>',
                ['somebody@localhost'],
                "Hello World",
            )

    asyncio.get_event_loop().run_until_complete(send_a_message())


And if you need a long-running connection:
    import asyncio
    import aiosmtplib

    smtp = aiosmtplib.AutoReconnectingSMTP(
        hostname='localhost',
        port=25,
        username='USERNAME', # optional
        password='PASSWORD',
    )

    asyncio.get_event_loop().run_until_complete(asyncio.gather(
        await smtp.sendmail('root@localhost', ['somebody@localhost'], "Hello world"),
        await smtp.sendmail('root@localhost', ['somebody@localhost'], "Goodbye world"),
    ))


Connecting to an SMTP server
----------------------------

Use an instance of the `SMTP` class to connect to a server.

Sending messages
----------------

Use ``SMTP.sendmail`` to send raw messages. The method signature is the same as
for standard smtplib.

Use ``SMTP.send_message`` to send ``email.message.Message`` objects. The method
signature is the same as for standard smtplib.
