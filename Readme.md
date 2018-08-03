# Logging macros for PostgreSQL with schemamacros

This is a simple module that allows for logging in PL/pgSQL code in the form
of `NOTICE` messages. There are log levels like those in Python. A lot more
could be done to make a comprehensible logging module for sure but this is more
of a showcase for `schemamacros` -packages at this point.

## Configuration in sm-config.yml
First off install this package into the current env. Then in the config simply
add the module name to `template_packages`:

```yaml

template_packages:
  - "schemamacros-logging"

```

Set the log levels accordingly for your different targes:

```yaml

targets:
  build/schema.out.sql:
    schema_template: "schema.sql"
    transaction: True
    variables:
      debug: True
      LOGGING_LEVEL: ERROR

  build/tests.out.sql:
    schema_template: "tests.sql"
    transaction: False
    variables:
      debug: True
      LOGGING_LEVEL: DEBUG

```

## Usage

Import the file `logging.sql` where you need it let it see the context for
log-level. And then you can call the logging macros where you want.

```jinja
{% import 'logging.sql' as log with context  %}


{{ log.CRITICAL("critical message") }}
{{ log.INFO("just some info") }}

```


If `LOGGING_LEVEL` is set so high that the message shall be omitted there will
not be a newline even in its place.

```
RAISE NOTICE 'CRITICAL: %', $LOG$critical message$LOG$;
```
