# Social Security Number (SSN) - Regex

In formal language theory, a regular expression (a.k.a. regex, regexp, or r.e.) is a string that represents a **regular (type-3) language**. 🧐 The only reason this is important is to note that the set of strings regular expressions are capable of matching goes way beyond what regular expressions from language theory can describe.

A Social Security Number (SSN) is a 9-digit number, typically formatted as three groups of numbers separated by hyphens: xxx-xx-xxxx.

There are approximately 453.7 million different sequences available for assignment. The Department of Social Security claims it uses 5.5. million new numbers annually, and statisticians say that the nine-digit SSN allows for approximately 1 billion possible combinations.

Assigned at birth, the SSN enables government agencies to identify individuals in their records and businesses to track financial information. Interestingly, the SSN sequences are not recycled according to the Department, and upon an individual's death the number is removed from the active files and is not reused. Recycling sequences may become an issue someday, and with analytics, we may be able to predict exactly when.

```diff
- Prior Limitations
```
Today, SSNs issues are random numbers, but that wasn't always the case. Before 2011, they were assigned regionally and in batches which limited the number of SSNs available for issuance to individuals by state. Before 2011, the nine-digit SSN was traditionally divided into 3 parts:

1) Area Numbers (*xxx*-xx-xxxx): first 3 digits originally represented the state in which a person first applied for a Social Security card.

2) Group Numbers (xxx-*xx*-xxxx): middle 2 digits were simply used to break all the SSNs with the same area number into smaller blocks.

3) Serial Numbers (xxx-xx-*xxxx*): last 4 digits within each group designation ran consecutively from 0001 through 9999.

> Speculation: ***more meaningful SSN sequences compromise randomization***

```diff
! Protecting the Integrity of Social Security Numbers
```

On July 3, 2007, the SSA published its intent to randomixe the nine-digit SSN in the Federal Register Notice, ***Protecting the Integrity of Social Security Numbers*** [Docket No. SSA 2007-0046]. SSN randomization affected the assignment process by:


- Eliminating the geographical significance of the first 3 digits of the SSN (area number) by no longer allocating by assignment to individuals in specific states
- Eliminating the significance of the highest group number and, making the High Group List forzen in time so it can only be used to see the area and group numbers SSA issued prior to randomization implementation
- Introducing previously unassigned area numbers exlucding 000, 666, and 900-999 (disadvantage)
- 

## Summary

A Regular Expression (regex) is a sequence of characters that define a search pattern. Regular expressions provide a generalized method to match patterns with diverse sequences.

Here, the task is to check whether a given string is a valid SSN or not by using Regular Expressions. The follow is a basic Regex pattern to verify that a given string is a valid SSN in this format:

> Example SSN match: 
>> 313-564-1837 
>> 000-482-9485

    `^\d{3}-\d{2}-\d{4}$`

        > Explanation:
        >> `^` matches the start of the string
        >> `\d{3}` matches three digits
        >> `-` matches a hyphen
        >> `\d{2}` matches two digits
        >> `-` matches another hyphen
        >> `\d{4}` matches four digits
        >> `$` matches the end of the string

While the regex above ***should*** technically match almost all SSNs, using this regex will still validate a small fraction of SSNs that include 000, 666, and digits between 900-999. According to the Department of Social Security, valid SSNs cannot have these digits in sequence. 

Below, is a regex that validates SSNs similarly to the regex above, but is a bit more accurate by excluding sequences with the first 3 digits equal to 000, 666, or between 900 and 999:

    ^(?!(000|666|9))\d{3}-(?!00)\d{2}-(?!0000)\d{4}$

        > Exaplanation: 
        >> `^` matches the start of the string
        >> `(?!...)` matches any 3-digits that are not followed by a specific string within `(?!...)`
        >> `(000|666|9)` excludes undesired values in the first group of 3 digits
        >> `\d{3}` matches three digits
        >> `-` matches a hyphen
        >> `(?!...)` matches any 2-digits that are not followed by a specific string within `(?!...)`
        >> `(..00)` excludes undesired values in the second group of 2 digits
        >> `\d{2}` matches two digits
        >> `-` matches another hyphen
        >> `(?!...)` matches any 3-digits that are not followed by a specific string within `(?!...)`
        >> `(..0000)` excludes undesired values in the last group of 4 digits
        >> `$` matches the end of the string


Overall, a valid SSN must satisfy the following conditions: 
1. It should have 9 digits
2. It should be divided into 3 parts by hyphen (-)
3. The first part (or group) should have 3 digits and should not be 000, 666, or beteen 900 and 999
4. The second part should have 2 digits and it should be from 01 to 99
5. The third part should have 4 digits and it should be from 0001 to 9999


> Just like writing any code, there are several ways to accomplish a successfull validation for Social Security Numbers. And... what if the SSNs are not all in the same format OR you want to change their format without doing so manually?! Let's discuss below!


## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components
Although their exact implementation can differ, components are generally characters that have a relatively universal use. Regular expression syntax is usually described in a grammatical form, but here we will describe it more loosly from bottom up, the way you would create one! At the bottom, are these basic components, also called atoms.



I am not sure if this will equate to the most ideal Big O Notation, I like to think of the process of regex matching like the process of DNA transcription and translation. I do believe human systems have almost perfected internal processes like DNA matchig, but we also unforuntely see mismatching, and it's consequences in cancer and other human mutations. This may be similar to where the regex passes through illegimate SSNs, even within a standard error rate. But I know human systems have multiple layers of validation, destructuring long sequences into digestable, matchable bits. Do regex have this ability to destructure, and potentially break validations into smaller bits in order to increase accuracy?... I don't know yet! 🙃

**match a regex with a target gene sequnce** 

- The enzyme (polymerase) that catalyze DNA synthesis in DNA replication goes through multiple sequences, while only matching and replicating the target DNA sequences

    -- like regex may run through several sequences and only matches one string

- Can be called from the 5' to 3' direction on either side of the double helix, and within different groups 
>> Does direction and segmented direction like during DNA replication with exponential growth in DNA strands upon splicing, increase the capacity for randomness of the random expression? 

- Initiated by start and stop sequences that are not variable amongst sequences

    -- like anchors, a non-unique regex copmonent

Can be affected by angles of interaction with the match, like the orientation of molecules or atoms (sub)

In moleclular biology and genetics, translation is the process of t

![Analogy to DNA Replication](assets/DNA-replication.png "Matching Codings Transcription and Translation")
![Codon Table Analogy to Anchors](assets/codon-sequences.png "Positional Matching")


- While explaining the following regex components and their relation to validating SSN, we will use the following sequence as an example:
    > `901-33-4539`

### Anchors
    > Special sequences which match an empty substring.
        >> THINK: position or format

Anchors, or atomic zero-width assertions, cause a match to succeed or fail depending on the current position in the string, but they do not cause the engine to advance through the string or consume characters. 



The following are the anchors used in the regex pattern for validating a Social Security Number (SSN):

| Symbol         | Name               | Use                              | Validating SSN                                  | Pattern | Matches |
|---------------:|:------------------:|:--------------------------------:|:-----------------------------------------------:|:-------:|:-------:|
| `^`            | `caret`            | Matches the beginning of the target string or the beginning of the line (in multiline mode) | Ensures that the pattern must start at the beginning of the string | `^\d{3}` | `901-...` |
| `$`            | `dollar sign`      | Matches the end of the target string (or line in multiline mode) or before `\n` at the end of the string (or line in multiline mode) | Ensures that the pattern must end at the end of string | `-\d{4}` | `...-4539` |


Other Anchors:
| Symbol         | Use                                         | Pattern | Example SSN regex | Example Match |
|---------------:|:-------------------------------------------:|:-------:|:-----------------:|:-------------:|
| `\A`           | The match must occur at the start of the string | `\A\d{3}` | `\A\d{3}`-\d{2}-\d{4}$ | 
| `\Z`           | The match must occur at the end of the string or before `\n` at the end of the string| `-\d{3}\Z` | ^\d{3}-\d{2}-\d{4}`\Z` | 
| `\z`           | The match must occur at the end of the string | `-\d{3}\z` | ^\d{3}-\d{2}-\d{4}`\z` |
| `\G`           | Matches either: 1) the beginning of the string 2) the position that immediately follows the end of the previous match | `\G\(\d\)` | | `"(1)", "(3)", "(5)" in "(1)(3)(5)[7](9)"` |
| `\b`           | The match must occur on a boundary between a `\w` (alphanumeric) and a `\W` (nonalphanumeric) character | `\b\w+\s\w+\b` | | `"them theme", "them them" in "them theme them them` | 
| `\B`           | The match must not occur on a `\b` boundary (inverse of `\b`) | `\Bend\w*\b` | |  `"ends", "ender" in "end sends endure lender"` | 

> The use of the `^` and `$` anchors in the SSN validation pattern ensures that the pattern mut match the entire string, not just a portion of it. This is important for ensuring that only valid SSN formats are considered matches, and not just strings that contain the correct number of digits in the right order.
>> Anchors can change the behavior of matching a string like start and stop codons specify where target DNA sequences (e.g. genes) can be matched by DNA polymerase.



### Quantifiers

    > Elements that specify how many instances of the previous element must be present in the input string for a match to occur
        >> THINK: occurances or frequency

Quantifiers in regular expressions (regex) specify how many times the preceding character, group, or character class shoud match. 


The follow are quantifiers used in the regex pattern for validating a SSN:

| Symbol       | Use                              | Validating SSN                                  | Matches         |
|-------------:|:--------------------------------:|:-----------------------------------------------:|:---------------:|
| `{n}`        | Matches the previous element exactly `n` times | `\d{3}`, `\d{2}`, and `\d{4}` | `901`, `33`, and `4539` |


- breaking this quantifier down for SSN validation

| Quantifier   | Use                  | SSN Validation            | Matches    | 
|:------------:|:--------------------:|:-------------------------:|:----------:|
| `\d{3}`      | Specifies that the preceding `\d` character class should match exactly three times | Used to match the first group of three digits in the SSN | `901-...` |
| `\d{2}`      | Specifies that the preceding `\d` character class should match exactly two times | Used to match the second group of two digits in the SSN | `...-33-...` |
| `\d{4}`      | Specifies that the preceding `\d` character class should match exactly four times | Used to match the third group of four digits in the SSN | `...-4539` |

> The use of these quantifiers in the SSN validation pattern ensures that only strings with the correct number of digits in each group will be considered matches
>> This helps to prevent invalid SSN formats, such as strings with too few or too many digits, from being considered valid


### Grouping Constructs

Grouping constructs in regular expressions are used to group parts of the pattern rogether and apply quantifiers or other operations to the entire group.

    > `^\d{3}-\d{2}-\d{4}$`

**There are no grouping constructs used in the standard SSN validation pattern shown above, as the pattern only consists of individual charactrers, character classes, quantifiers, and anchors. This is because the pattern is used to match the entire string, so it is not neccessary to group portions of the pattern together.**

- BUT how can we apply Grouping Constructs to SSN regex validation? 
    - Let's think of a real-life example!

    > Example: I have hundreds of SSNs and want to find and replace all

Regular expression search:
- Assigns full match to `Group 0`
- () means that that part of the full match should be captured separatetly as a different group
    - Groups numbered left to right, Group 0 being the full match, Group 1 being the captured group farthest to the left, Group 2 being the captured group located to the right of Group 1, etc.


In general, the main grouping constructs used in regex are:



| Symbol         | Name               | Use                              | Pattern | Matches | Potential regex using Grouping Constructs |  |
|---------------:|:------------------:|:--------------------------------:|:-------:|:-------:|:------------------------:|:---------:|
| `()`           | Capturing Group    | Used to group parts of the pattern together and capture the matched text. The captured sequence can be accessed through group references, such as `/1`, `/2`, et., Within the group, you can apply quantifiers,alternation, and other operations to the entire group. | `(\w+\s)+` | Matches one or more word characters followed by a whitespace, repeated one or more times | `(\d{3}-\d{2}-\d{4}\s)+` | Let's say you had several SSN numbers separated single whitespaces. Instead of the `^` and `$` anchors defining the acceptable start and end matches for one sequence, this may be able to separate and validate several SSN numbers placed together. |
| `(?:)`         | Non-capturing Group | Similar to capturing groups, but do not capture the text matched by the group. This is useful when applying operations to a group of characters but do not need to access (or consume) the matched text. |



### Bracket Expressions

### Character Classes
| Symbol         | Use                              | Pattern | Matches |
|---------------:|:--------------------------------:|:-------:|:-------:|
| `\d`, `\D`     | Any one digit/non-digit character | Digits are [0-9] | |
| `\w`, `\W`     | Any one word/non-word charactrer | For ASCII, word characters are [a-zA-Z0-9_] | |
| `\s`, `\S`     | Any one space/non-space charactrer | for ASCII, whitespace charactrers are [ \n\r\t\f] | |


### The OR Operator

### Flags

### Character Escapes


## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
