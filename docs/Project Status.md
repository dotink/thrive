## PORTING OVERVIEW AND PROGRESS

### Completed Classes

Completed classes are those which have had all porting done and have additionally been brought into check with the current coding standards.  A class which is completed should be considered the first initial version with the exception of oversights and/or bugs.

- Message
- Text (Includes Grammar)
- JSON
- URL
- ExpectedException
- AuthorizationException
- ConnectivityException
- ContinueException
- EnvironmentException
- NoRemainingException
- NotFoundException
- ProgrammerException
- SQLException
- YieldException

### Working Classes

Working classes are those which have had initial porting done, but are not necessarily cleaned up or completely in line with current coding standards.  These will generally work and may be relied upon by other classes, but need thorough review and cleanup.  Their API is subject to change.

- Core
- UTF8
- Filesystem
- File
- Directory
- Date
- Time
- Timestamp
- Exception
- UnexpectedException

### Needs Review

These are classes which have been submitted via pull request and are likely working but have not had any extensive review done.  These will move to either "Working" or "Completed" once they have been initially reviewed.

- Email

### MIA

Classes which have been "ported" somewhere, to what extent we're not sure as people have not submitted the pull request yet.

- Cryptography

### Normal Priority

We welcome ports of these classes that are in line with the goals and coding standards of FlourishToo

- Cookie
- Image

### Low Priority

In some cases we don't care too much about a class at all.  Port this if you need it and want to, but we frankly don't care.

- Buffer
- HTML

### Fuck It

In other cases, we actually want to prevent a class from making it's way back in here.

- CRUD
- EmptySetException

### On Hold

- Cache
- Authorization
- Mailbox
- Database
- ActiveRecord
- Schema


## Class Specifics

Below you will find specific ideas for various classes.  If you do not find a class here it is either because we significantly encourage more discussion on this class or related classes before moving forward, because we have no intention to use it or deal with it at the time, or because it is already completed.

### Cookie

Cookie should be turned into an object where the name is the common constructor value.  The concept below shows what this might look like for getting a cookie.

```php
$cookie = (new Cookie('name'))->get('integer', 0, TRUE);
```

Set would be similar API.

```php
$cookie = (new Cookie('name'))->set($value, $expires, $path);
```

All the static methods for setting default paths, etc, can reman static.

### Cryptography

Cryptography should be made into an object whose primary constructor argument is the data which it is concerned with.  See the concepts below.

### Hash a Password

The default algorithm, if not specified should begin using the stronger bcrypt hashing mechanisms.  A 'compat' algorithm should be added for Flourish's traditional stretched SHA1 hashes.

```php
$hash = (new Cryptogrpahy($password))->hash('algorithm')
```

### Random

Random strings should be generated from the pool of provided data.  The below for example would generate a random string of 10 characters from the `$pool`.  The second parameter which traditionally defined the pool will instead define a filter.  If the `$pool` is `NULL` there could be a built in default which would contain a-z, A-Z, 0-9, and various special characters.

```php
$random = (new Cryptography($pool))->randomString(10, $filter = 'hexidecimal')
```

```php
$random = (new Cryptography())->randomNumber(10, 100)
```

### Encrypt a String

```php
$encrypted_data = (new Cryptography($text))->symmetricKeyEncrypt($key);
```

### Decrypt a String

```php
$data = (new Cryptography($encrypted_data))->symmetricKeyDecrypt($key);
```

### Date

We should look into having Date, Time, and Timestamp based on PHP's DateTime.

### Grammar

Grammar functionality has been merged with Text

### HTML (NULL)

HTML should function similar to JSON where you can pass a string to the HTML object.  The HTML object itself should be a DOM driven object of sorts whereby you can manipulate the HTML you just seeded it with.  From here you could also `compose()` after some manipulation or `escape()` which will compose but also replace special characters.

```
$string = (new HTML($string))->escape();
```

Stupid helpers like printOption should burn in hell and be left to a separate helper class somewhere.


