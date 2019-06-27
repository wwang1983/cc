# ABNF

## Naming

ABNF - Augmented Backus-Naur Form

## Rule Definitin

### Rule Naming

The name of a rule is simple the name itself; beginning with an alphabetic character, and followed by a combination of alphabetics, digits and hyphens (dashes).

Rule names are case-insensitive.

Unlike original BNF, angle brackets("<",">") are not required.

### Rule Form

A rule is defined by the following sequence:

```ABNF
name = elements crlf
```

where <name> is the name of the rule, <elements> is one or more rule names or terminal specifications and <crlf> is the end-of-line indicator, carriage return followed by line feed. The equal sign separates the name from the definition of the rule. The elements form a sequence of one or more rule names and/or value definitions, combined according to the various operators, such as alternative and repetition.

For visual ease, rule definition are left aligned. When a rule requires multiple lines, the continuation lines are indented. The left alignment and indentation are relative to the first lines of the ABNF rules and need not match the left margin of the document.

### Terminal Values

Rules resolve into a string of terminal values, sometimes called characters.  In ABNF a character is merely a non-negative integer. In certain contexts a specific mapping (encoding) of values into a character set (such as ASCII) will be specified.

Terminals are specified by one or more numeric characters with the base interpretation of those characters indicated explicitly.  The following bases are currently defined:

```ABNF
b = binary
d = decimal
x = hexadecimal
```

Hence:

```ABNF
CR = %d13
CR = %x0D
```

respectively specify the decimal and hexadecimal representation of [US-ASCII] for carriage return.

A concatenated string of such values is specified compactly, using a period (".") to indicate separation of characters within that value. Hence:

```ABNF
CRLF = %d13.10
```

ABNF permits specifying literal text string directly, enclosed in quotation-marks.  Hence:

```ABNF
command = "command string"
```

Literal text strings are interpreted as a concatenated set of printable characters.
NOTE : ABNF strings are case-insensitive and the character set for these strings is us-ascii.
Hence:

```ABNF
rulename = "abc"
```

and 

```ABNF
rulename = "aBc"
```

will match "abc", "Abc", "aBc", "abC", "ABc", "aBC", "AbC" and "ABC".

To specify a rule which is case SENSITIVE, specify the characters individually.

For example:

```ABNF
rulename = %d97 %d98 %d99
```

or

```ABNF
rulename = %d97.98.99
```

will match only the string which comprises only lowercased character, abc.

### External Encodings

External representations of terminal value characters will vary according to constraints in the storage or transmission environment. Hence, the same ABNF-based grammar may have multiple external encodings, such as one for a 7-bit US-ASCII environment, another for a binary octet environment and still a different one when 16-bit Unicode is used.  Encoding details are beyond the scope of ABNF, although Appendix A (Core) provides definitions for a 7-bit US-ASCII environment as has been common to much of the Internet.

By separating external encoding from the syntax, it is intended that alternate encoding environments can be used for the same syntax.

## Operators

### Concatenation    Rule1 Rule2

A rule can define a simple, ordered string of values -- i.e., a concatenation of contiguous characters -- by listing a sequence of rule names.  For example:

```ABNF
foo = %x61 ;a
bar = %x62 ;b
mumble = foo bar foo
```

So that the rule <mumble> matches the lowercase string "aba".

### Alternatives   Rule1 / Rule2

Elements separated by forward slash ("/") are alternatives.
Therefore,

```ABNF
foo / bar
```

will accept <foo> or <bar>.

NOTE: A quoted string containing alphabetic characters is special form for specifying alternative characters and is interpreted as a non-terminal representing the set of combinatorial strings with the contained characters, in the specified order but with any mixture of upper and lower case..

### Incremental Alternatives   Rule1 =/ Rule2

It is sometimes convenient to specify a list of alternatives in fragments.  That is, an initial rule may match one or more alternatives, with later rule definitions adding to the set of alternatives.  This is particularly useful for otherwise- independent specifications which derive from the same parent rule set, such as often occurs with parameter lists.  ABNF permits this incremental definition through the construct:

```ABNF
oldrule =/ additional-alternatives
```

So that the rule set

```ABNF
ruleset = alt1 / alt2
ruleset =/ alt3
ruleset =/ alt4 / alt5
```

is the same as specifying

```ABNF
ruleset = alt1 / alt2 / alt3 / alt4 / alt5
```

### Value Range Alternatives   %c##-##

A range of alternative numeric values can be specified compactly, using dash ("-") to indicate the range of alternative values.  Hence:

```ABNF
DIGIT = %x30-39
```

is equivalent to :

```ABNF
DIGIT =  "0" / "1" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9"
```

Concatenated numeric values and numeric value ranges can not be specified in the same string.  A numeric value may use the dotted notation for concatenation or it may use the dash notation to specify one value range.  Hence, to specify one printable character, between end of line sequences, the specification could be:

```ABNF
char-line = %x0D.0A %x20-7E %x0D.0A
```

### Sequence Group   (Rule1 Rule2)

Elements enclosed in parentheses are treated as a single element, whose contents are STRICTLY ORDERED.   Thus,

```ABNF
elem (foo / bar) blat
```

which matches (elem foo blat) or (elem bar blat).

```ABNF
elem foo / bar blat
```

matches (elem foo) or (bar blat).

NOTE: It is strongly advised to use grouping notation, rather than to rely on proper reading of "bare" alternations, when alternatives consist of multiple rule names or literals.

Hence it is recommended that instead of the above form, the form:

```ABNF
(elem foo) / (bar blat)
```

be used. It will avoid misinterpretation by casual readers.

The sequence group notation is also used within free text to set off an element sequence from the prose.

### Variable Repetition   *Rule

The operator "*" preceding an element indicates repetition. The full form is:

```ABNF
<a>*<b>element
```

where **\<a\>** and **\<b\>** are optional decimal values, indicating at least **\<a\>** and at most **\<b\>** occurences of elements.

Default values are 0 and infinity so that **\*\<element\>** allows any number, including zero; **1\*\<element\>** requires at least one; **3\*3\<element\>** allows exactly 3 and **1\*2\<element\>** allows one or two.

### Specific Repetition   nRule

A rule of the form

```ABNF
<n>element
```

is equivalent to 

```ABNF
<n>*<n>element
```

That is, exactly <N> occurences of <element>. Thus 2DIGIT is a 2-digit number, and 3ALPHA is a string of three alphabetic characters.

### Optional Suquence   [RULE]

Square brackets enclose an optional element sequence:

```ABNF
[foo bar]
```

is equivalent to

```ABNF
*1(foo bar)
```

### Comment   ;

A semi-colon starts a comment that continues to the end of the line.
This is a simple way of including useful notes in parallel with the specifications.

### Operator Precedence

The various mechanisms described above have the following precedence, from highest (binding tightest) at the top, to lowest and loosest at the bottom:

```ABNF
Strings, Names formation
Comment
Value range
Repetition
Grouping, Optional
Concatenation
Alternative
```

## ABNF Definition of ABNF

This syntac uses the rules provided in Appendix A (Core).

```ABNF
rulelist       =  1*( rule / (*c-wsp c-nl) )
rule           =  rulename defined-as elements c-nl
                               ; continues if next line starts
                               ;  with white space
rulename       =  ALPHA *(ALPHA / DIGIT / "-")
defined-as     =  *c-wsp ("=" / "=/") *c-wsp
                               ; basic rules definition and
                               ;  incremental alternatives
elements       =  alternation *c-wsp
c-wsp          =  WSP / (c-nl WSP)
c-nl           =  comment / CRLF
                               ; comment or newline
comment        =  ";" *(WSP / VCHAR) CRLF
alternation    =  concatenation *(*c-wsp "/" *c-wsp concatenation)
concatenation  =  repetition *(1*c-wsp repetition)
repetition     =  [repeat] element
repeat         =  1*DIGIT / (*DIGIT "*" *DIGIT)
element        =  rulename / group / option / char-val / num-val / prose-val
group          =  "(" *c-wsp alternation *c-wsp ")"
option         =  "[" *c-wsp alternation *c-wsp "]"
char-val       =  DQUOTE *(%x20-21 / %x23-7E) DQUOTE
                               ; quoted string of SP and VCHAR without DQUOTE
num-val        =  "%" (bin-val / dec-val / hex-val)
bin-val        =  "b" 1*BIT [ 1*("." 1*BIT) / ("-" 1*BIT) ]
                               ; series of concatenated bit values
                               ; or single ONEOF range
dec-val        =  "d" 1*DIGIT
                          [ 1*("." 1*DIGIT) / ("-" 1*DIGIT) ]
hex-val        =  "x" 1*HEXDIG
                          [ 1*("." 1*HEXDIG) / ("-" 1*HEXDIG) ]
prose-val      =  "<" *(%x20-3D / %x3F-7E) ">"
                               ; bracketed string of SP and VCHAR without angles
                               ; prose description, to be used as last resort
```

## APPENDIX A - CORE

```ABNF
ALPHA          =  %x41-5A / %x61-7A                            ; A-Z / a-z
BIT            =  "0" / "1"
CHAR           =  %x01-7F                                      ; any 7-bit US-ASCII character, excluding NUL
CR             =  %x0D                                         ; carriage return
CRLF           =  CR LF                                        ; Internet standard newline
CTL            =  %x00-1F / %x7F                               ; controls
DIGIT          =  %x30-39                                      ; 0-9
DQUOTE         =  %x22                                         ; " (Double Quote)
HEXDIG         =  DIGIT / "A" / "B" / "C" / "D" / "E" / "F"
HTAB           =  %x09                                         ; horizontal tab
LF             =  %x0A                                         ; linefeed
LWSP           =  *(WSP / CRLF WSP)                            ; linear white space (past newline)
OCTET          =  %x00-FF                                      ; 8 bits of data
SP             =  %x20                                         ; space
VCHAR          =  %x21-7E                                      ; visible (printing) characters
WSP            =  SP / HTAB                                    ; white space
```

## Reference

<https://www.ietf.org/rfc/rfc2234.txt>
