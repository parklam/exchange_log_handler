# exchange_log_handler
Log handler to send log via exchange

## Dependencies
This library used [ecederstrand/exchangelib](https://github.com/ecederstrand/exchangelib) to send exchange mail.

## How to install:
`
pip install exchange-log-handler
`
## Usage:
```
if __name__ == '__main__':
    import logging
    import logging.config
    logging.config.dictConfig({
        'version': 1,
        'disable_existing_loggers': False,
        'handlers': {
            'console': {
                'level': 'INFO',
                'class': 'logging.StreamHandler',
                'stream': 'ext://sys.stdout',
            },
            'email': {
                'level': 'ERROR',
                'class': 'exchange_log_handler.ExchangeHandler',
                'credentials': ('email_address', 'email_password'),
                'subject': lambda r: '{0}-{1}'.format(r.levelname, r.name),
                'toaddrs': 'recipient_email_address',
            },
        },
        'loggers': {
            '': {
                'handlers': [ 'console', 'email' ],
                'level': 'DEBUG',
                'propagate': True,
            },
        },
    })

    logger = logging.getLogger(__name__)
    try:
        raise ValueError('This is a test exception.')
    except Exception as e:
        logger.exception(e)
```
