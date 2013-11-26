*NOTE: This is just as spec. If you're interested in working on this with me,
email me at lamp.greg@gmail.com*

# A spec for an unnamed language that makes R more familiar looking

## Coffee-Script for R
A language that allows you to take advantage of the vastness of R but that
makes the syntax a little bit easier to stomach.

## Variable Assignment
Only accept `=`

## Data Frames
Allow "dot" access to columns
```
df = data.frame(x=1:100, y=1:100)
df.one = 1
```

Less verbose subsetting and selecting of data
```
df[x > 100, ]
df[x > 100 & y < 10, ]
df[, ['x', 'y']]
df[x > 100, ['x', 'y']]
```

Negative Indexing
```
df[-10:-1,]
# returns last 10 rows of data frame
```
*We would have to automatically convert the negatives to positives. Probably by 
looking at then lengt of the dataframe and then calculating the positive 
indices.*

Plus equals, times equals, etc.
```
df.one *= -1
df.one /= -1
df.one == -1
```
*These would just be a sort of function(?)*

## Scoping
Allow scope to be defined by indentation instead of brackets
```
for i in 1:100:
  print(i)
```
*Grammar that parses based on indentation rather than {}*

## Looping
For loops should be more arbitrary like Python or Ruby
```
for i in seq(1, 100):
  print(i)
```
*Grammar based. Should work w/ scoping.*

Sequences like Ruby
```
for i in 0..100:
  print(i)
```
*Not sure if this is totally neccessary. `seq` does a pretty good job at this.*


## Lists
Define true lists as you would in Python or Ruby. Lists should be able
to take arbitrary types but should have a type. So something with strings
and list would become an object but you can still operate the column.
```
myList = [1, 2, 3]
```
*Grammar based. Could be tricky if it starts to conflict with subsetting. We should
be able to take a chunk of code that is blah[] and identify it as subsetting, while
something that had no blah would be a list.*

## Dicts
A dictionary like Python or Ruby. Simple key/value pairs.
```
x = {"one": 1, "two": 2, "three", "We're really doing it Harry!"}
x['one']
```
*This will be 2 part. We'll need to use the dicts from my R package (probably?) and 
also handle the syntax in the grammar. Shouldn't be too bad since we're getting rid
of {} anyways.*

## Functions
Slightly more sugary function definition. Also need to figure out what `...` does
and then make it easier to use.
```
func hello():
  return "Hello, Greg!"
```
*Grammar based. Maybe `def` instead of `func`?*

## Classes
More sane class structures. Allow users to define classes with properties, 
initializers, etc.
```
class MyObject(object):
  func init(self, something):
    self.something = something
  func equals(self, aMyObject):
    return self.something==aMyObject.something
  func hello(self, name):
    print("Hello!", name)
```
*This could be tricky. We might have to convert each of these into the shitty R 
Reference Classes. Largely this will need to be done via the grammar with a lot of
helper functions being generated. Should be a 2nd tier priority feature since OOP
in R isn't that neccessary.*

Creating objects should be like Ruby/Python.
```
myObj = MyObject("something wicked")
```
*Via the grammar. I think it can just behave like a special function.*

## Docstrings
Docstring support like Python for functions and classes.
```
func hello(somebody):
  """
  Says hello to somebody

  params:
    somebody - someone who wants to be greeted
  """
  return "Hello! " + somebody
```
*Can handle this in the grammar for functions/classes. Can just ignore the 
strings.*

## Package Manager
R needs a better package manager. CRAN is great until it's a pain. A less 
stringent, easier to use package manager could really help. Peole are already
thinking about this [packrat](https://github.com/rstudio/packrat) and I'm seeing
more and more R packages NOT distributed on CRAN in favor of just using github
in conjunction with `devtools::install_github`.

```bash
    $ rpm install foo
    $ rpm freeze > packages.txt
    $ rpm install packages.txt
```

## Strings and output
Strings in R are totally bizarre. You should be able to concatenate strings by
adding them together. Adding a string with a non-string (a number) should invoke
the `toString()` on the non-string and add them together.
```
'x' + 'y' =='xy'
```
*Grammar based. Will default this to `paste(x, y)`.*

Printing also needs to do what you think it should. 
```
print("hello", "greg")
# hello greg
```
*Wrapper around `print`. Can just use some sort of xargs-like feature. I think `...`.*

Block string support
```
x = """this
is
a
multi-line
string
"""
```
*Grammar based. Automatically escaping ".*

## Single-String
It would be cool to have a new data-type that is a string but that is implicit 
that it's not a vector. You could give it OOP-like properties.
```
x = "my single-string"
length(x)!=1
```
*Would probably need to write a new class to do this. Tier 2 priority.*

Substrings need to be easier to be better defined. We could get sugary with 
strings and have some psuedo-OOP like this
```
x = "hello"
x.slice(1, 3)
# hel
x.slice(-3, -1)
# llo
```
*Would probably need to write a new class to do this. Tier 2 priority.*

