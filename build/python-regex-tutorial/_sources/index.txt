=============================
Regular Expressions in Python
=============================

.. Here is were you specify the content and order of your new book.

.. Each section heading (e.g. "SECTION 1: A Random Section") will be
   a heading in the table of contents. Source files that should be
   generated and included in that section should be placed on individual
   lines, with one line separating the first source filename and the
   :maxdepth: line.

.. Sources can also be included from subfolders of this directory.
   (e.g. "DataStructures/queues.rst").

Introduction
------------

If there’s one thing that humans do well, it’s pattern matching.
You can categorize the numbers in the following list with barely any
thought:

::
    
    321-40-0909
    302-555-8754
    3-15-66
    95135-0448

You can tell at a glance which of the following words can’t possibly
be valid English words by the pattern of consonants and vowels:
    
::
    
    grunion vortenal pskov trebular elibm talus

**Regular expressions** are Python’s method of letting your program
look for patterns:

- A fraction is a series of digits followed by a slash, followed by another series of digits.
- A valid name consists of a series of letters, a comma followed by zero or more spaces, followed by another series of letters.

The Simplest Patterns
---------------------

To use regular expressions in python, you must import the **r**\ egular **e**\ xpression module with ``import re``.
The simplest pattern to look for is a single letter. If you want to
see if a variable ``word`` contains the letter ``e``, for
example, you can use this code:

.. activecode:: re_example1
   :language: python
   :caption: Simple regular expression

   import re

   word = input('Enter a string: ')
   found = re.search(r'e', word)
   if found:
       print(word, 'contains the letter "e".')
   else:
       print(word, 'does not contain the letter "e".')


The pattern is a string preceded by the letter ``r``, which tells Python to interpet the string as a regular expression.

Of course, you can put more than one letter in your pattern. You can
look for the word ``eat`` anywhere in a word. This example uses a function
that is called repeatedly for a series of words.

.. activecode:: re_example2
   :language: python
   :caption: Another simple regular expression

   import re
   def find_eat(word):
       found = re.search(r'eat', word)
       if found:
           print(word, 'contains the letters "eat".')
       else:
           print(word, 'does not contain the letters "eat".')
   
   find_eat('heater')
   find_eat('treat')
   find_eat('easy')
   find_eat('metal')
   
This will successfully match the words *eat*, *heater*, and
*treat*, but won’t match *easy*, *metal*, or
*hat*. You may be saying, "So what? I can do the same thing with
the ``find()`` string function."  Yes, you can, but now let’s do
something that isn’t so easy to do with ``find()``:


Matching any single character
-----------------------------

Let’s make a pattern that will match the letter ``e``
followed by *any character at all*, followed by the letter
``t``. To say “any character at all”, you use a dot.
Here’s the pattern:

.. activecode:: re_example3
   :language: python
   :caption: Finding any single character

   import re
   def finder(pattern, word):
       found = re.search(pattern, word)
       if found:
           print(word, 'contains the pattern.')
       else:
           print(word, 'does not contain the pattern.')
   
   finder(r'e.t', 'better')
   finder(r'e.t', 'either')
   finder(r'e.t', 'best')
   finder(r'e.t', 'beast')
   finder(r'e.t', 'etch')
   finder(r'e.t', 'ease')

   
This will match *better*, *either*, and *best*
(the dot will match the *t*, *i*, and *s* in those
words).  It will not match *beast* (two letters between the *e*
and *t*), *etch* (no letters between the *e* and
*t*), or *ease* (no letter *t* at all!).


Matching classes of characters
==============================

Now let’s find out how to narrow down the field a bit. We’d like to be
able to find a pattern consisting of the letter *b*, any vowel
(*a*, *e*, *i*, *o*, or *u*), followed
by the letter *t*.  To say "any one of a certain series of
characters", you enclose them in square brackets:

.. activecode:: re_example4
   :language: python
   :caption: Finding one of a set of characters

   import re
   def finder(pattern, word):
       found = re.search(pattern, word)
       if found:
           print(word, 'contains the pattern.')
       else:
           print(word, 'does not contain the pattern.')
   
   finder(r'b[aeiou]t', 'bat')
   finder(r'b[aeiou]t', 'bet')
   finder(r'b[aeiou]t', 'rabbit')
   finder(r'b[aeiou]t', 'robotic')
   finder(r'b[aeiou]t', 'abutment')
   finder(r'b[aeiou]t', 'boot')
   finder(r'b[aeiou]t', 'beautiful')


This matches words like *bat*, *bet*, *rab*\ **bit**,
*ro*\ **bot**\ *ic*, and *a*\ **but**\ *ment*.  It won’t match
*boot* or *beautiful*, because there is more than one letter between the *b* and
*t*, and the class matches only a single character. (You’ll see how
to check for multiple vowels later.)


There are abbreviations for establishing a series of letters:
``[a-f]`` is the same as ``[abcdef]``;
``[A-Gm-p]`` is the same as ``[ABCDEFGmnop]``;
``[0-9]`` matches a single digit (same as ``[0123456789]``).


You may also complement (negate) a class; the next
two patterns will look for the letter
*e* followed by anything **except** a vowel, followed by
the letter *t*; or any character **except** a capital letter:

::
    
    r'e[^aeiou]t'
    r'[^A-Z]'



There are some classes that are so useful that Python provides
quick and easy abbrevations:

+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+
| Abbreviation              | Means                                                                        | Same as                                   |
+===========================+==============================================================================+===========================================+
| ``\d``                    | a digit                                                                      | ``[0-9]``                                 |
+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+
| ``\w``                    | a "word" character; uppercase letter, lowercase letter, digit, or underscore | ``[A-Za-z0-9_]``                          |
+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+
| ``\s``                    | a whitespace character (blank, new line, tab)                                | ``[ \r\t\n]``                             |
+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+
| And their complements:                                                                                                                               |
+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+
| ``\D``                    | a non-digit                                                                  | ``[^0-9]``                                |
+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+
| ``\W``                    | a non-word character                                                         | ``[^A-Za-z0-9_]``                         |
+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+
| ``\S``                    | a non-whitespace character                                                   | ``[^ \r\t\n]``                            |
+---------------------------+------------------------------------------------------------------------------+-------------------------------------------+

Thus, this pattern: ``\d\d\d-\d\d-\d\d\d\d`` matches a Social Security number; again, you’ll see a shorter way later on.

    
.. activecode:: re_example5
   :language: python
   :caption: Finding a social security number

   import re
   def find_ssn(in_str):
       found = re.search(r'\d\d\d-\d\d-\d\d\d\d', in_str)
       if found:
           print(in_str, 'contains a social security number.')
       else:
           print(in_str, 'does not contain a social security number.')
   
   find_ssn('301-22-0156') # these are all made-up numbers
   find_ssn('301-555-1212')
   find_ssn('SSN is 562-99-6713')
   

Anchors
-------

All the patterns youe’ve seen so far will find a match anywhere within
a string, which is usually - but not always - what you want.  For example,
you might insist on a capital letter, but only as the very first character
in the string. Or, you might say that an employee ID number has to end
with a digit. Or, you might want to find the word *go* only if it
is at the beginning of a word, so that you will find it in *You met another, and pfft you was*  **go**\ *ne.*, but
you won’t mistakenly find it
in *I for*\ **go**\ *t my umbrella*. This is the purpose of
an anchor; to make sure that you are at a certain boundary before you
continue the match. Unlike character classes, which match individual
characters in a string, these anchors do not match any character; they
simply establish that you are on the correct boundaries.


The up-arrow ``^`` matches the beginning of a line, and
the dollar sign ``$`` matches the end of a line.  Thus,
``^[A-Z]`` matches a capital letter at the beginning of the
line. Note that if you put the ``^`` *inside* the
square brackets, that would mean something entirely different!
A pattern ``\d$`` matches a digit at the end of a line.
These are the boundaries you will use most often.

The other two anchors are ``\b`` and ``\B``, which stand for
a "word boundary" and "non-word boundary".  For example, if you want
to find the word *met* at the beginning of a word, we write
the pattern ``r'\bmet``, which will match *The metal plate* and *The metropolitan lifestyle*,
but not *Wear your bike helmet*.
The pattern ``r'ing\b'`` will match *Hiking is fun*
and *Reading, writing, and arithmetic*, but not *Gold ingots
are heavy*.  Finally,the pattern ``r'/\bhat\b`` matches only
the *The hat is red* but not *That is the question* or *she hates anchovies* or *the shattered glass*.


.. activecode:: re_example6
   :language: python
   :caption: Word boundary test

   import re
   def find_boundary(in_str):
       found1 = re.search(r'\bmet', in_str)
       found2 = re.search(r'ing\b', in_str)
       found3 = re.search(r'\bhat\b', in_str)
       if found1:
           print(in_str, 'has "met" at the start of a word.')
       if found2:
           print(in_str, 'has "ing" at the end of a word.')
       if found3:
           print(in_str, 'has the word "hat" in it.')
           
   in_str = input('Enter one of the preceding sentences:')
   find_boundary(in_str)


While ``\b`` is used to find the breakpoint between words and
non-words, ``\B`` finds pairs of letters or nonletters;
``/\Bmet/`` and ``/ing\b/`` match the opposite
examples of the preceding paragraph;
``/\Bhat\B/`` matches only *the shattered glass*.


Repetition
----------

All of these classes match only one character; what if we want to
match three digits in a row, or an arbitrary number of vowels?
You can follow any class or character by a repetition count:

+---------------------------+-------------------------------------------------+
| Pattern                   | Matches                                         |
+===========================+=================================================+
| ``r'b[aeiou]{2}t'``       | ``b`` followed by two vowels, followed by ``t`` |
+---------------------------+-------------------------------------------------+
| ``r'A\d{3,}'``            | The letter ``A`` followed by 3 or more digits   |
+---------------------------+-------------------------------------------------+
| ``r'[A-Z]{,5}'``          | Zero to five capital letters                    |
+---------------------------+-------------------------------------------------+
| ``r'\w{3,7}'``            | Three to seven "word" characters                |
+---------------------------+-------------------------------------------------+

This lets you rewrite the social security number pattern match
as ``r'\d{3}-\d{2}-\d{4}'``

There are three repetitions that are so common that Python has
special symbols for them: ``*`` means “zero or more,”
``+`` means “one or more,” and
``?`` means “zero or one.”  Thus, if you want to look
for lines consisting of last names followed by a first initial,
you could use this pattern:

.. activecode:: re_example7
   :language: python
   :caption: Finding last name and initial

   import re
   def find_last_and_initial(in_str):
       found = re.search(r'^\w+,?\s*[A-Z]$', in_str)
       if found:
           print(in_str, 'contains the pattern.')
       else:
           print(in_str, 'does not contain the pattern.')
   
   find_last_and_initial('Smith, J')
   find_last_and_initial('M Cano')
   find_last_and_initial('Nguyen C')


Let’s analyze that pattern by parts:
    
* ``^`` starting at the beginning of the string,
* ``\w+`` look for one or more word characters
* ``,?`` followed by an optional comma (zero or one commas)
* ``\s*`` zero or more spaces
* ``[A-Z]`` and a capital letter
* ``$`` which must be at the end of the string.

Grouping
--------

So far so good, but what if you want to scan for a last name,
followed by an optional comma-whitespace-initial; thus matching only a
last name like "Smith" or a full "Smith, J"?  You need to put the
comma, whitespace, and initial into a unit with parentheses, and then
follow it with a ``?`` to indicate that the whole group is optional.

.. activecode:: re_example8
   :language: python
   :caption: Using a group

   import re
   def valid_name(in_str):
       found = re.search(r'^\w+(,?\s*[A-Z])?$', in_str)
       if found:
           print(in_str, 'contains the pattern.')
       else:
           print(in_str, 'does not contain the pattern.')
   
   valid_name('Smith, J')
   valid_name('Madonna')
   valid_name('Morgan D')


Note: If you want to match a parenthesis in your pattern, you have to precede it
with a backslash to make it non-special.


Modifiers
---------

If you want a pattern match to be case-insenstive, add the
``flags=re.I`` to the ``search()`` call. (The ``I`` stands for
``IGNORECASE``, which you may also spell out completely if you wish.
The following example shows a pattern that will match any Canadian postal
code in upper or lower case. Canadian postal codes consist of a letter, a digit,
and another letter, followed by a space, a digit, a letter, and another digit.
An example of a valid postal code would be ``A5B 6R9``. Here is what the code
looks like; it does not work in ActiveCode, but will work fine in IDLE.

::
   
   import re
   def valid_postcode(in_str):
       found = re.search(r'^[A-Z]\d[A-Z]\s+\d[A-Z]\d$', in_str, flags=re.I)
       if found:
           print(in_str, 'is a valid postal code')
       else:
           print(in_str, 'is not a valid postal code.')
   
   valid_postcode('A5B 6R9')
   valid_postcode('c7H 8j2')


At this point, you know pretty much everything you need to test whether a string
matches a particular pattern.


Advanced Pattern Matching
-------------------------

All you have done so far is testing to see whether a pattern matches
or not.  Now that you can match a person’s last name and initial, you
might want to be able to grab them out of the string so that you can
change <i>Martinez, A</i> to <i>A. Martinez</i>. To accomplish this,
you will need something other than ``search()``; you will need the
``sub()`` method to do substitution. You will also have to use
the grouping parentheses, which have a
side effect:  whenever you use parentheses to
group something, the pattern matching operation stores the part of the
string that matched the group so that you can use it later on.

It is now time to reveal a secret: the return value from ``search()`` is not a boolean; it is a
*matching object* that has special properties that you can examine and use. In the following example,
we have put parentheses around the “last name” part of the pattern as well as the “comma and
initial” part. If there is a match, the program will display whatever was found in the
grouping parentheses. The vertical bars are in the ``print()`` so that you can see
where there are blanks (if any).

.. activecode:: re_example9
   :language: python
   :caption: Seeing Contents of a Group

   import re
   def valid_name(in_str):
       found = re.search(r'^(\w+)(,?\s*[A-Z])?$', in_str)
       if found:
           print('Pattern match results:')
           print('whole match:      |', found.group(0), '|', sep='')
           print('first set of ():  |', found.group(1), '|', sep='')
           print('second set of (): |', found.group(2), '|', sep='')
       else:
           print(in_str, 'does not contain the pattern.')

   valid_name('Smith, J')
   # valid_name('Madonna')  # try uncommenting these as well
   # valid_name('Morgan D')


In the preceding code, ``found`` is the match object produced by the
``search()`` method. The ``found.group(0)`` method calls contains everything
matched by the entire pattern. ``found.group(1)`` contains the part of the string that the
first set of grouping parentheses matched--the last name, and ``found.group(2)`` contains
the part of the string matched by the second set of grouping parentheses--the comma and
initial, if any. If the pattern had more groups of parentheses, you would use ``.group(3)``,
``.group(4)``, and so forth.

If you look at the output from <kbd>Smith, J</kbd> you’ll see that the
second set of grouping parentheses doesn’t give you quite what you want.
The group stores the entire matched substring, which includes the comma. You’d like to
store only the initial. You can do this two ways.  First, you can
include yet another set of parentheses:


.. activecode:: re_example10
   :language: python
   :caption: Using Nested Groups

   import re
   def valid_name(in_str):
       found = re.search(r'^(\w+)(,?\s*([A-Z]))?$', in_str)
       if found:
           print('Pattern match results:')
           print('whole match:     |', found.group(0), '|', sep='')
           print('first set of (): |', found.group(1), '|', sep='')
           print('outer set of (): |', found.group(2), '|', sep='')
           print('inner set of (): |', found.group(3), '|', sep='')
       else:
           print(in_str, 'does not contain the pattern.')

   valid_name('Smith, J')

If you do it this way, then the capital letter is stored in
``found.group(3)`` and the entire comma-and-initial is stored in ``found.group(2)``.


The other way to do this is to say that the outer parentheses should group but
*not* store the matched portion in the
result array. You do that with a question mark and colon right after
the outer parentheses.

.. activecode:: re_example11
   :language: python
   :caption: Using Non-storing Groups

   import re
   def valid_name(in_str):
       found = re.search(r'^(\w+)(?:,?\s*([A-Z]))?$', in_str)
       if found:
           print('Pattern match results:')
           print('whole match:     |', found.group(0), '|', sep='')
           print('first set of (): |', found.group(1), '|', sep='')
           print('inner set of (): |', found.group(2), '|', sep='')
       else:
           print(in_str, 'does not contain the pattern.')

   valid_name('Smith, J')

In this case, the initial is in  ``found.group(2)``, since the outer set of open parentheses
doesn’t get stored. As you can see, patterns can
very quickly become difficult to read, so let’s break this into parts:
    
*  ``^`` at the start of the string,
* ``(\w+)`` look for (and remember) one or more “word” characters
* ``(?:``  start a non-storing group which consists of:
         
  - ``,?`` an optional comma
  - ``\s*`` zero or more spaces
  - ``([A-Z])`` and a capital letter, which is remembered because it is in parentheses
  
* ``)?`` this ends the non-storing group; the question mark means it is all optional
* ``$`` at which point we must be at the end of the string.

Now that you know how to extract the last name and initial, you are in a position to use ``sub()`` to
swap their positions. The ``re.sub()`` method takes three arguments:
    
- A pattern to search for
- A replacement pattern
- The string to search in

So, ``re.sub(r'-\d{4}', r'-XXXX', '301-22-0109')`` will replace the last four digits of a Social Security
number by Xes. This example does not work in ActiveCode (since ``re.sub()`` is not implemented), but it will work in IDLE.
    
::

   import re
   result = re.sub(r'-\d{4}', r'-XXXX', '301-22-0109')
   print(result)


If you are using grouping, you can use ``\1`` and ``\2`` in the replacement pattern to stand for
the first and second matched groups. This is how you can write a program that will change names
like “Gonzales, M” to “M. Gonzales”; in the following example, the comma and initial are *not* optional.

::
    
   import re
   def swap_name(in_str):
       result = re.sub(r'^(\w+),?\s*([A-Z])$', r'\2. \1', in_str )
       return result
       
   print(swap_name('Smith, J'))
   print(swap_name('Joe-Bob Smythe-Fauntleroy'))
   print(swap_name('Madonna'))
   print(swap_name('Gonzales M'))
    
If you run the preceding program in IDLE, you will see that if the pattern does not match, ``re.sub()`` returns a copy of the input string, untouched..

Finally, another example with groups. Say you want to match a phone number and find the area code,
prefix, and number. In this case, rather than doing a substitution, we return a list with the relevant information, or a list of three empty strings
if the input is not valid. Note that when you want to match to a real parenthesis, you have to precede it with a backslash to make
it “not part of a group.” You can do it this way:

.. activecode:: re_example12
   :language: python
   :caption: Phone number with groups

   import re
   def valid_phone(in_str):
       """Return a list giving the area code, prefix, and number;
       if not a valid number, return empty strings for all three."""
       found = re.search(r'\((\d{3})\)\s*(\d{3})-(\d{4})', in_str)
       if found:
          area_code = found.group(1)
          prefix = found.group(2)
          number = found.group(3)
          result = [area_code, prefix, number]
       else:
          result = ['', '', '']
       return result

   data = valid_phone('(408) 555-1212')
   print(data)

Again, let’s break apart that pattern:
    
* ``\(`` look for a real open parenthesis
* ``(\d{3})`` followed by three digits (and store them)
* ``\)`` and a real closing parenthesis
* ``\s*`` followed by zero or more spaces
* ``(\d{3})`` three digits (store them)
* ``-`` a dash
* ``(\d{4})`` and four more digits (store them)


Finding All Occurrences
-----------------------

The ``re.search()`` method finds only the first occurrence of a pattern within a string.
If you want to find *all* the matches in a string, use ``re.findall()``, which returns a
list of matched substrings. (Unlike ``re.search()``, which returns match objects.)
Here is a pattern that finds a capital letter followed by an optional
dash and a single digit:

::
    
    r'([A-Z]-?\d)'
    

Let’s match to find all the occurrences of this pattern in the following string:

::
    
   'Insert tabs B3, D-7, and C6 into slot A9.'

.. activecode:: re_example13
   :language: python
   :caption: Finding all occurrences

   import re
   message = 'Insert tabs B3, D-7, and C6 into slot A9.'
   result = re.findall(r'([A-Z]-?\d)', message)
   if result:
       for item in result:
           print(item)
   else:
       print('findall() did not find any matches to the pattern.')

Conclusion
----------

This tutorial has shown you some of the main points of pattern matching. To learn all the ins and outs of
regular expressions, see the `Python documentation <https://docs.python.org/3/library/re.html>`_

Once you learn regular expressions, you will be able to use them in many text editors. For example, the
“Replace” and “Find” dialogs in IDLE have a checkbox that let you use regular expressions to find and replace text.
