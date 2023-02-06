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
- Introducing previously unassigned area numbers exlucding 000, 666, and 900-999

## Summary

A Regular Expression (regex) is a sequence of characters that define a search pattern. Regular expressions provide a generalized method to match patterns with diverse sequences.

Here, the task is to check whether a given string is a valid SSN or not by using Regular Expressions. The follow is a basic Regex pattern to verify that a given string is a valid SSN in this format:

    `^\d{3}-\d{2}-\d{4}$`

        > Explanation:
        >> `^` matches the start of the string
        >> `\d{3}` matches three digits
        >> `-` matches a hyphen
        >> `\d{2}` matches two digits
        >> `-` matches another hyphen
        >> `\d{4}` matches four digits
        >> `$` matches the end of the string


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



I am not sure if this will equate to the most ideal Big O Notation, I like to think of the process of regex matching like the process of DNA transcription and translation. I do believe human systems have almost perfected internal processes like DNA matchig, but we also unforuntely see mismatching, and it's consequences in cancer and other human mutations. This may be similar to where the regex passes through illegimate SSNs, even within a standard error rate. But I know human systems have multiple layers of validation, destructuring long sequences into digestable, matchable bits. Do regex have this ability to destructure, and potentially break validations into smaller bits to increase accuracy?... I don't know yet! 🙃

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
| `\B`           | The match must not occur on a `\b` boundary | `\Bend\w*\b` | |  `"ends", "ender" in "end sends endure lender"` | 

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


>> breaking this quantifier down for SSN validation

| Quantifier   | Use                  | SSN Validation            | Matches    | 
|:------------:|:--------------------:|:-------------------------:|:----------:|
| `\d{3}`      | Specifies that the preceding `\d` character class should match exactly three times | Used to match the first group of three digits in the SSN | `901-...` |
| `\d{2}`      | Specifies that the preceding `\d` character class should match exactly two times | Used to match the second group of two digits in the SSN | `...-33-...` |
| `\d{4}`      | Specifies that the preceding `\d` character class should match exactly four times | Used to match the third group of four digits in the SSN | `...-4539` |

> The use of these quantifiers in the SSN validation pattern ensures that only strings with the correct number of digits in each group will be considered matches
>> This helps to prevent invalid SSN formats, such as strings with too few or too many digits, from being considered valid


### Grouping Constructs

### Bracket Expressions

### Character Classes

### The OR Operator

### Flags

### Character Escapes

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
