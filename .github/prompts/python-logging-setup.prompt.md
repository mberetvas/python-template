# Logging Implementation Checklist

**Directive:** Implement a robust logging configuration for a Python application using the standard `logging` module.

**Requirements:**
- Configure a rotating file handler (`RotatingFileHandler`) to manage log file size.
- Use a dictionary-based configuration (`logging.config.dictConfig`).
- Define separate logger configurations for development (DEBUG level) and production (INFO level).
- Create a standardized log message format that includes a timestamp, log level, module name, and message.
- Add a filter or wrapper to inject a correlation ID for tracking requests across services.
- Show an example of how to automatically log unhandled exceptions using `sys.excepthook`.
