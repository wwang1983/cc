# CSV

## The definition of CSV file

1 Each record is located on a separate line, delimited by a line break (CRLF).  For example:

``` csv
aaa,bbb,ccc CRLF
zzz,yyy,xxx CRLF
```

2 The last record in the file may or may not have an ending line break.  For example:

``` csv
aaa,bbb,ccc CRLF
zzz,yyy,xxx
```

3 There maybe an optional header line appearing as the first line of the file with the same format as normal record lines.  This  header will contain names corresponding to the fields in the file and should contain the same number of fields as the records in the rest of the file (the presence or absence of the header line should be indicated via the optional "header" parameter of this MIME type).  For example:

``` csv
field_name,field_name,field_name CRLF
aaa,bbb,ccc CRLF
zzz,yyy,xxx CRLF
```

4 Within the header and each record, there may be one or more fields, separated by commas.  Each line should contain the same number of fields throughout the file.  Spaces are considered part of a field and should not be ignored.  The last field in the record must not be followed by a comma.  For example:

```csv
aaa,bbb,ccc
```

5 Each field may or may not be enclosed in double quotes (however some programs, such as Microsoft Excel, do not use double quotes at all).  If fields are not enclosed with double quotes, then double quotes may not appear inside the fields.  For example:

```csv
"aaa","bbb","ccc" CRLF
zzz,yyy,xxx
```

6 Fields containing line breaks (CRLF), double quotes, and commas should be enclosed in double-quotes.  For example:

```csv
 "aaa","b CRLF
bb","ccc" CRLF
zzz,yyy,xxx
```

7 If double-quotes are used to enclose fields, then a double-quote appearing inside a field must be escaped by preceding it with another double quote.  For example:

```csv
"aaa","b""bb","ccc"
```

## The ABNF grammar appears as follows

```ABNF
file = [header CRLF] record *(CRLF record) [CRLF]

header = name *(COMMA name)

record = field *(COMMA field)

name = field

field = (escaped / non-escaped)

escaped = DQUOTE *(TEXTDATA / COMMA / CR / LF / 2DQUOTE) DQUOTE

non-escaped = *TEXTDATA

COMMA = %x2C

CR = %x0D ;as per section 6.1 of RFC 2234 [2]

DQUOTE =  %x22 ;as per section 6.1 of RFC 2234 [2]

LF = %x0A ;as per section 6.1 of RFC 2234 [2]

CRLF = CR LF ;as per section 6.1 of RFC 2234 [2]

TEXTDATA =  %x20-21 / %x23-2B / %x2D-7E
```

## Reference

[RFC4180]{<https://tools.ietf.org/html/rfc4180}>

