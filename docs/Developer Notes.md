# Developer Notes

## Coding Standards

FlourishToo uses different coding standards from Flourish Beta/1.x.  Please keep these in mind if you are developing or porting.

### Margin

Set your editor margin to 100 characters.  The only line that should exceeed this is a method signature in the event it has a lot of parameters and/or default valued parameters.

If the logic of a method is too long at certain points you should break it down into additional private methods to handle some of that logic.  Use semantic names for these methods.

### Whitespace

Use tabs for indentation and spaces for alignment.  An extra new line should be used between blocks of assignments vs. conditional blocks, vs. a series of method calls, etc.

Excess whitespace should **not** be used to align docblock comments.

### Array Syntax

Use array() if you are initializing an empty array.  Use [] if you are initializing or assigning an array with values.

### Naming

- Classes should be UpperCamelCase.
- All class properties should be lowerCamelCase.
- All class methods should be lowerCamelCase.
- All variables inside a method should be under_score.
- Functions should not exist.

### 'Final', 'Abstract', 'Static', Access Keywords

From left to right a method or property signature should appear as follows:

`<final|abstract> <static> <public|protected|private> lowerCamelCaseName...`

The access keyword `public` must be specified if the property or method is public even though it will be the default.

### Order of Properties and Methods

Properties and Methods should be ordered in the following manner:

- All properties should precede methods
- Within properties and methods, all static elements should precede object elements
- All `public` should precede `protected`, and all `protected` should precede `private`
- Within each group of `public`, `protected`, and `private` elements should be ordered alphabetically
- There is no restriction on `final`/`abstract`

```
                            [public]    - [alphabetical]
                          /
                 [static] - [protected] - [alphabetical]
               /          \
              /             [private]   - [alphabetical]
[properties] -
              \             [public]    - [alphabetical]
               \          /
                 [object] - [protected] - [alphabetical]
                          \
                            [private]   - [alphabetical]


                            [public]    - [alphabetical]
                          /
                 [static] - [protected] - [alphabetical]
               /          \
              /             [private]   - [alphabetical]
[methods] ----
              \             [public]    - [alphabetical]
               \          /
                 [object] - [protected] - [alphabetical]
                          \
                            [private]   - [alphabetical]

```


## Porting from Flourish Beta/1.x

There are several common changes which need to be made when porting from Flourish Beta or 1.x.

- Add namespace 'Dotink\Flourish'.
- Change static class properties to lowerCamelCase notation from under_score notation
- Remove "clean callback" definitions.
- Fix "clean callback" calls (They must be converted to strings)
- Reorder class elements (properties and methods) according to coding standards.
- Fix spacing, broken margins, etc, according to coding standards.
- Remove any logic designed to cope with anything less than PHP 5.4

### Rewriting

While FlourishToo does not intend on being a clean room rewrite.  That is, we are still using much of the code from Flourish Beta/1.x, we do intend to change the API and also introduce some sanity with regards to the use of static classes.

Much of how we want to change Flourish is documented in the README file currently.  You can use
that for guidance.  However, if an existing class is not in there, please add a description of the changes you propose for that class in the README and create a pull request.

### On Dependencies

We are trying to change how dependencies are managed in Flourish without introducing an explcit service locator or container class.  We **do** accept static dependencies for the `Core` and `UTF8` class as well as any exceptions.  All other dependencies should be optional or injected.

If you note any extremely common logic across various classes, we accept proposals for adding to core.  One example of this is a `compose()` method which is added to many classes to handle composing text.  This method usually checks to see if the `Text` class is loaded and will use that if it is or normal `vsprintf()` otherwise.

You should keep an eye out for such pieces which have been migrated to `Core` and fix them.