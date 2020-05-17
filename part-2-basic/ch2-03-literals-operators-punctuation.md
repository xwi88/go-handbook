# 2.3 字面量、操作符与标点字符

## 2.3.1 字面量

### 整型字面量

整形字面量是一系列描述整数常量的数字。用于设置非十进制数的前缀有:

- 二进制, **0b or 0B**
- 八进制，**0, 0o, or 0O**
- 十六进制, **0x or 0X**, `a->f` 或 `A->F` 表示十进制的 `10->15`

>为了可读性，下划线`_`可以出现在表示基数的前缀之后或连续数字之间;这样的下划线不会改变字面量的值。

```text
int_lit        = decimal_lit | binary_lit | octal_lit | hex_lit .
decimal_lit    = "0" | ( "1" … "9" ) [ [ "_" ] decimal_digits ] .
binary_lit     = "0" ( "b" | "B" ) [ "_" ] binary_digits .
octal_lit      = "0" [ "o" | "O" ] [ "_" ] octal_digits .
hex_lit        = "0" ( "x" | "X" ) [ "_" ] hex_digits .

decimal_digits = decimal_digit { [ "_" ] decimal_digit } .
binary_digits  = binary_digit { [ "_" ] binary_digit } .
octal_digits   = octal_digit { [ "_" ] octal_digit } .
hex_digits     = hex_digit { [ "_" ] hex_digit } .
42
4_2
0600
0_600
0o600
0O600       // second character is capital letter 'O'
0xBadFace
0xBad_Face
0x_67_7a_2f_cc_40_c6
170141183460469231731687303715884105727
170_141183_460469_231731_687303_715884_105727

_42         // an identifier, not an integer literal
42_         // invalid: _ must separate successive digits
4__2        // invalid: only one _ at a time
0_xBadFace  // invalid: _ must separate successive digits
```

### 浮点型字面量

浮点字面量是浮点常量的十进制或十六进制表示。一个十进制浮点字面量包含:十进制的整数部分，十进制的小数点位，十进制的小数部分，十进制的指数部分(e或者E后紧跟可选的符号及十进制数字)。

整数部分或小数部分中的一部分可以省略;小数部分或指数部分可以省略，指数值exp将尾数(整数和小数部分)乘以10exp。

十六进制浮点文字由一个0x或0x前缀、一个整数部分(十六进制数字)、一个基数、一个小数部分(十六进制数字)和一个指数部分(p或p后跟一个可选符号和十进制数字)组成。整数部分或小数部分中的一部分可以省略;基数也可以省略，但是需要使用指数部分。(此语法与IEEE 754-2008 中给出的语法相匹配。)指数值exp将尾数(整数和小数部分)乘以2exp。

>为了可读性，下划线`_`可以出现在表示基数的前缀之后或连续数字之间;这样的下划线不会改变字面量的值。

```text
float_lit         = decimal_float_lit | hex_float_lit .

decimal_float_lit = decimal_digits "." [ decimal_digits ] [ decimal_exponent ] |
                    decimal_digits decimal_exponent |
                    "." decimal_digits [ decimal_exponent ] .
decimal_exponent  = ( "e" | "E" ) [ "+" | "-" ] decimal_digits .

hex_float_lit     = "0" ( "x" | "X" ) hex_mantissa hex_exponent .
hex_mantissa      = [ "_" ] hex_digits "." [ hex_digits ] |
                    [ "_" ] hex_digits |
                    "." hex_digits .
hex_exponent      = ( "p" | "P" ) [ "+" | "-" ] decimal_digits .
0.
72.40
072.40       // == 72.40
2.71828
1.e+0
6.67428e-11
1E6
.25
.12345E+5
1_5.         // == 15.0
0.15e+0_2    // == 15.0

0x1p-2       // == 0.25
0x2.p10      // == 2048.0
0x1.Fp+0     // == 1.9375
0X.8p-0      // == 0.5
0X_1FFFP-16  // == 0.1249847412109375
0x15e-2      // == 0x15e - 2 (integer subtraction)

0x.p1        // invalid: mantissa has no digits
1p-2         // invalid: p exponent requires hexadecimal mantissa
0x1.5e-2     // invalid: hexadecimal mantissa requires p exponent
1_.5         // invalid: _ must separate successive digits
1._5         // invalid: _ must separate successive digits
1.5_e1       // invalid: _ must separate successive digits
1.5e_1       // invalid: _ must separate successive digits
1.5e1_       // invalid: _ must separate successive digits
```

### 虚数字面量

虚数字面量表示复数常量的虚部。它由一个整型或浮点型文字加上小写的字母i组成。一个虚数字面量的值是相应的整型或浮点型字面量乘以虚数单位i的值。

**`imaginary_lit =(十进制数字| int_lit | float_lit) "i"`**

为了向后兼容，一个虚数字面量的整数部分(完全由十进制数字(可能还有下划线)组成)被认为是十进制整数，即使它以`前导0`开头。

>为了可读性，下划线`_`可以出现在表示基数的前缀之后或连续数字之间;这样的下划线不会改变字面量的值。

```text
0i
0123i         // == 123i for backward-compatibility
0o123i        // == 0o123 * 1i == 83i
0xabci        // == 0xabc * 1i == 2748i
0.i
2.71828i
1.e+0i
6.67428e-11i
1E6i
.25i
.12345E+5i
0x1p-2i       // == 0x1p-2 * 1i == 0.25i
```

### Rune 字面量

一个rune字面量代表一个rune常量，一个识别`Unicode`编码点的整数值。一个`rune字面量`被表达为一个或多个单引号括起来的字符，如‘x’或‘\n‘。在引号中，除了换行和未转义的单引号外，任何字符都可以出现。单引号字符表示字符本身的Unicode值，而以反斜杠开头的多字符序列以多种格式编码值。

Rune字面量最简单的形式是表示单引号内的字符;**因为Go源文本是用UTF-8编码的Unicode字符**，_所以多个UTF-8编码的字节可以表示一个整数值_。例如，文字`'a'`保存一个表示文字a的字节，Unicode U+0061，值0x61，而`'ä'`保存两个字节(0xc3 0xa4)，表示一个字面量`a-dieresis`, U+00E4，值0xe4。

几个反斜杠转义允许将任意值编码为ASCII文本。
有四种方法可以将整数值表示为数字常量:

- `\x`后跟两个十六进制数字;
- `u`后跟4个十六进制数字;
- `\U`后跟8个十六进制数字；
- 一个简单的反斜杠`\`后跟3个八进制数字。

在每一种情况下，字面量的值是由相应基数中的数字表示的值。

尽管这些表示的结果都是整数，但它们的有效范围不同。八进制转义必须表示0到255之间的值(包括0和255)。十六进制转义通过构造满足此条件。转义`\u`和`\U`表示Unicode编码点，因此在它们中间有些值是非法的，特别是那些位于`0x10FFFF`和**surrogate halves**(代理部分之上的值?)。

>在反斜杠之后，某些单字符转义表示特殊的值:

```text
\a   U+0007 alert or bell
\b   U+0008 backspace
\f   U+000C form feed
\n   U+000A line feed or newline
\r   U+000D carriage return
\t   U+0009 horizontal tab
\v   U+000b vertical tab
\\   U+005c backslash
\'   U+0027 single quote  (valid escape only within rune literals)
\"   U+0022 double quote  (valid escape only within string literals)
All other sequences starting with a backslash are illegal inside rune literals.

rune_lit         = "'" ( unicode_value | byte_value ) "'" .
unicode_value    = unicode_char | little_u_value | big_u_value | escaped_char .
byte_value       = octal_byte_value | hex_byte_value .
octal_byte_value = `\` octal_digit octal_digit octal_digit .
hex_byte_value   = `\` "x" hex_digit hex_digit .
little_u_value   = `\` "u" hex_digit hex_digit hex_digit hex_digit .
big_u_value      = `\` "U" hex_digit hex_digit hex_digit hex_digit
                           hex_digit hex_digit hex_digit hex_digit .
escaped_char     = `\` ( "a" | "b" | "f" | "n" | "r" | "t" | "v" | `\` | "'" | `"` ) .
'a'
'ä'
'本'
'\t'
'\000'
'\007'
'\377'
'\x07'
'\xff'
'\u12e4'
'\U00101234'
'\''         // rune literal containing single quote character
'aa'         // illegal: too many characters
'\xa'        // illegal: too few hexadecimal digits
'\0'         // illegal: too few octal digits
'\uDFFF'     // illegal: surrogate half
'\U00110000' // illegal: invalid Unicode code point
```

### 字符串字面量

字符串字面量表示从连接字符序列获得的字符串常量。有两种形式:`原始字符串文本`和`解释字符串文本`。

**原始字符串(Raw string)字面量**是反引号(back quote)之间的字符序列，如在\`foo\`。在引号内，**除了反引号外**，任何字符都可以出现。原始字符串字面量的值是由引号之间的未解释(隐式`UTF-8`编码)字符组成的字符串;特别是，反斜杠没有特殊的含义，字符串可能包含换行。原始字符串字面量中的回车字符('\r')将从原始字符串值中丢弃。

**解释字符串字面量**是`双引号`之间的字符序列，如`"bar"`。在引号中，**除了换行和未转义的双引号外**，任何字符都可以出现。引号之间的文本形成字面量的值，
用反斜杠转义解释为它们在rune字面量中(除了\`是非法的而\"是合法的)，具有相同的限制。三位八进制(\nnn)和两位十六进制(\xnn)转义表示产生的字符串的各个字节;
所有其他转义都表示单个字符的(可能是多字节的)UTF-8编码。因此，在字符串字面量 `\377`和`\xFF`中表示值`0xFF=255`的单个字节，而`ÿ`、`\u00FF`、`\U000000FF`和`\xc3\xbf`表示字符`U+00FF`的UTF-8编码的两个字节`0xc3 0xbf`。

```text
string_lit             = raw_string_lit | interpreted_string_lit .
raw_string_lit         = "`" { unicode_char | newline } "`" .
interpreted_string_lit = `"` { unicode_value | byte_value } `"` .
`abc`                // same as "abc"
`\n
\n`                  // same as "\\n\n\\n"
"\n"
"\""                 // same as `"`
"Hello, world!\n"
"日本語"
"\u65e5本\U00008a9e"
"\xff\u00FF"
"\uD800"             // illegal: surrogate half
"\U00110000"         // illegal: invalid Unicode code point
These examples all represent the same string:

"日本語"                                 // UTF-8 input text
`日本語`                                 // UTF-8 input text as a raw literal
"\u65e5\u672c\u8a9e"                    // the explicit Unicode code points
"\U000065e5\U0000672c\U00008a9e"        // the explicit Unicode code points
"\xe6\x97\xa5\xe6\x9c\xac\xe8\xaa\x9e"  // the explicit UTF-8 bytes
```

如果源代码将字符表示为两个代码点，例如包含重音符号和字母的组合形式，则如果将其放置在rune字面量中(不是单个代码点)，结果将是错误的，如果放在字符串字面量中，将显示为两个代码点。

## 2.3.2 操作符与标点字符

```text
+    &     +=    &=     &&    ==    !=    (    )
-    |     -=    |=     ||    <     <=    [    ]
*    ^     *=    ^=     <-    >     >=    {    }
/    <<    /=    <<=    ++    =     :=    ,    ;
%    >>    %=    >>=    --    !     ...   .    :
     &^          &^=
```
