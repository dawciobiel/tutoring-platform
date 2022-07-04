# Logger

## Log4j 2

### This table below should guide you on which log4j2 level should be used in which case.

| Level  | Log Event Level                                                                         |
|--------|-----------------------------------------------------------------------------------------|
| OFF    | When no events will be logged                                                           |
| FATAL  | When a severe error will prevent the application from continuing                        |
| ERROR  | When an error in the application, possibly recoverable                                  |
| WARN   | When an event that might possible lead to an error                                      |
| INFO   | When an event for informational purposes                                                |
| DEBUG  | When a general debugging event required                                                 |
| TRACE  | When a fine grained debug message, typically capturing the flow through the application |
| ALL    | When all events should be logged                                                        |

### We mainly have 3 components to work with Log4j

**Logger**: It is used to log the messages.  
**Appender**: It is used to publish the logging information to the destination like the file, database, console etc.  
**Layout**: It is used to format logging information in different styles.

### Create a Log Directory

Create directories for the log files.

Oracle recommends the directory/file permission should be (rw-,r---). Only the install owner and group should be allowed
to read these files due to the sensitive nature of the information that could be contained within them.

### Pre-requisites for Email Alerts

For email alerts to work, third-party libraries must be copied into the `WEB-INF/lib` folder.
The required files are _activation.jar_, which can be downloaded from

    http://java.sun.com/javase/technologies/desktop/javabeans/jaf/index.jsp

_mail.jar_, which can be downloaded from

    http://java.sun.com/products/javamail/

## Log4j 2 configuration: XML vs JSON?

There is no performance advantage or disadvantage to using XML or JSON. Personally I prefer XML for these two reasons: (
but they are not huge problems so of course it is up to you...)

- JSON configuration requires the Jackson jar files in the classpath, so there's an extra dependency. XML uses the XML
  parser built in to Java.
- All examples in the log4j2 manual use XML, not JSON, so you'll need to convert the syntax. If you choose XML you can
  copy-and-paste from the manual.

## Log4j2 configuration format with best coverage

Log4j2 currently supports 4 configurations formats: XML, JSON, YAML, and properties syntax.
Is it possible to use all features of Log4j2 in every format or there are certain formats that lack some expressiveness?

The answer to the question is that from a Log4j perspective they are all equivalent. But from a user's perspective they
are not.

1. In general, XML files allow anything that can be expressed as an XML attribute to also be specified as an XML
   element.
   So on PatternLayout you can do either:
    ```xml
    
    <PatternLayout pattern="%m%n"/>
    ```

   or

    ```xml
    
    <PatternLayout>
        <Pattern>%m%n</Pattern>
    </PatternLayout>
    ```

2. The log4j configuration is hierarchical. For example, all Logger elements are configured under the Loggers element.
   Specific configuration for a Logger is inside that Logger element. XML, JSON, and YAML are all hierarchical by
   nature.
   Properties are not. To emulate that hierarchy you have to prepend every element with the name of the element that
   precedes it in the tree. This can get messy, which is why I never use the properties syntax and resisted implementing
   it
   in the first place.
3. The properties format is the only format that doesn't require any dependencies outside of java.base (in Java 9+
   terms).
   Unfortunately, XML is not part of the minimal Java distribution in Java 9+ and JSON/YAML never were.

So yes, you should be able to express any configuration in any of the formats, but figuring out what it should be in the
properties format sometimes requires expressing it in one of the other formats just so you can figure out what the
property equivalent syntax should be.

## Any reasons to use programmatic approach instead of a static configuration file?

I would not recommend programmatic configuration. It is more complex and depends on implementation specifics, which may
change in the future. IMHO it will be a bunch of work for little or no benefit.

## Links

- [Setting Up Logging - docs.oracle.com](https://docs.oracle.com/cd/E12057_01/doc.1014/e12050/logging.htm)
- [How Log4J2 Works](https://stackify.com/log4j2-java/)
- [Log4j 2 Configuration](https://logging.apache.org/log4j/2.x/manual/configuration.html)
- [Spring Profiles, different Log4j2 configs](https://stackoverflow.com/questions/35559824/spring-profiles-different-log4j2-configs)