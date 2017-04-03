<!--- @file
  3 Expression Format

  Copyright (c) 2014-2017, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

-->

# 3 Expression Format

The following EBNF describes the syntax and precedence of EDK II meta-data
expression notation.

1. An Expression is a sequence of operators and operands that specifies a
   computational value, or that designates an object (or a function), or that
   performs a combination thereof.

2. Statements in nested parenthesis are evaluated inside to outside.

3. Statements in parenthesis are evaluated prior to evaluating statements
   outside of parenthesis.

4. Operators within a grouping (as designated by the name preceding the "::="
   character sequence) are evaluated left to right.

5. The syntax is listed in precedence order (high to low).

6. The syntax does not define acceptable values of a data field; those are
   defined in the EDK II meta-data documents that define the data fields.

## 3.1 Data Field Expression

The following expression syntax may be used in a data field in EDK II Meta-data
documents. All text inclusive of a semi-colon character through the end of the
line must be viewed as a comment to the content preceding the semi-colon
character. The use of a semi-colon character as a comment delimiter is for this
specification only; other specifications may use different characters as
comment delimiters.

#### Restrictions

**`<Function>`**

_FUNCTIONS SHOULD ONLY BE USED IF ALL TOOLS THAT PROCESS THE ENTRY IN THE
META-DATA FILE COMPREHEND THE FUNCTION SYNTAX AND THE FUNCTION ELEMENT HAS
BEEN DEFINED IN A HIGH LEVEL DESIGN DOCUMENT OR SOFTWARE ARCHITECTURE
SPECIFICATION_.

_IF A TOOL DOES NOT COMPREHEND THE FUNCTION FORMAT, THE TOOL SHOULD FAIL
GRACEFULLY._

#### Prototype

```c
<PrimaryExpression>     ::= {<Identifier>} {<Constant>} {<StringLiteral>} {<Function>}
                            {"(" <Expression> ")"} {<Expression>}
<Identifier>            ::= {<CName>} {<MACROVAL>}
<MACROVAL>              ::= "$(" <MACRO> ")"
<MACRO>                 ::= (A-Z) [(A-Z0-9_)]*
<Function>              ::= <CName> "(" <Argument> [<CSP> <Argument>]* ")"
<Argument>              ::= <PrimaryExpression>
<CName>                 ::= (a-zA-Z_) [(a-zA-Z0-9_)]*
<Constant>              ::= {<TrueFalse>} {<Number>} {<GuidValue>} {<Array>}
<TrueFalse>             ::= {<True>} {<False>}
<True>                  ::= {"TRUE"} {"True"} {"true"}
<False>                 ::= {"FALSE"} {"False"} {"false"}
<Number>                ::= {<Integer>} {<HexNumber>}
<Integer>               ::= <Base10>
<Base10>                ::= {(0-9)} {(1-9) [(0-9)]*}
<HexNumber>             ::= <Base16>
<Base16>                ::= <HexPrefix> [<HexDigit>]+
<HexDigit>              ::= (a-fA-F0-9)
<HexPrefix>             ::= {"0x"} {"0X"}
<GuidValue>             ::= {<RformatGuid>} {<CformatGuid>}
Rhex2                   ::= [<HexDigit>] <HexDigit>
Rhex4                   ::= [<HexDigit>] [<HexDigit>] Rhex2
Rhex8                   ::= [<HexDigit>] [<HexDigit>] [<HexDigit>] [<HexDigit>] Rhex4
<RformatGuid>           ::= Rghex8 "-" Rghex4 "-" Rghex4 "-" Rghex4 "-" Rghex12
Rghex2                  ::= <HexDigit> <HexDigit>
Rghex4                  ::= <HexDigit> <HexDigit> Rghex2
Rghex8                  ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit> Rghex4
Rghex12                 ::= <HexDigit> <HexDigit> <HexDigit> <HexDigit> Rghex8
<Byte>                  ::= <HexPrefix> Rhex2
<Hex16>                 ::= <HexPrefix> Rhex4
<Hex32>                 ::= <HexPrefix> Rhex8
<CSP>                   ::= 0x2c [<TSP>]*
<TSP>                   ::= {0x20} {0x09}
<CformatGuid>           ::= "{" [<TSP>]* <Hex32> <CSP> <Hex16> <CSP> <Part2>
<Part2>                 ::= <Hex16> <CSP> "{" [<TSP>]* <Byte> <CSP> <Part3>
<Part3>                 ::= <Byte> <CSP> <Byte> <CSP> <Byte> <CSP> <Part4>
<Part4>                 ::= <Byte> <CSP> <Byte> <CSP> <Byte> <CSP> <Part5>
<Part5>                 ::= <Byte> [<TSP>]* "}" [<TSP>]* "}"
<Array>                 ::= {<EmptyArray>} {<Array>}
<EmptyArray>            ::= "{" <TSP>* "}"
<ByteArray>             ::= "{" <TSP>* <Byte> [<CSP> <Byte>]* "}"
<StringLiteral>         ::= {<QuotedString>} {"L" <QuotedString>}
<DblQuote>              ::= 0x22
<QuotedString>          ::= <DblQuote> [<CCHAR>]* <DblQuote>
<CCHAR>                 ::= {<SingleChars>} {<EscapeCharSeq>}
<SingleChars>           ::= {0x20} {0x21} {(0x23 - 0x5B)} {(0x5D - 0x7E)}
<EscapeCharSeq>         ::= "\" {"n"} {"r"} {"t"} {"f"} {"b"} {"0"} {"\"} {<DblQuote>}
<PostFixExpression>     ::= {<PrimaryExpression>} {<PcdName>}
<PcdName>               ::= <CName> "." <CName>
<UnaryExpression>       ::= {<PostFixExpression>} {<UnaryOp> <UnaryExpression>}
<UnaryOp>               ::= {<IntegerOps>} {<ScalarOps>}
<IntegerOps>            ::= {"+"} {"-"} {"~"}
<ScalarOps>             ::= {"NOT"} {"not"} {"!"}
<MultiplicativeExpress> ::= {<UnaryExpression>}
                            {<MultiplicativeExpress> <TSP>* "*" <TSP>* <UnaryExpression>}
                            {<MultiplicativeExpress> <TSP>* "/" <TSP>* <UnaryExpression>}
                            {<MultiplicativeExpress> <TSP>* "%" <TSP>* <UnaryExpression>}
<AdditiveExpress>       ::= {<MultiplicativeExpress>}
                            {<AdditiveExpress> <TSP>* "+" <TSP>* <MultiplicativeExpress>}
                            {<AdditiveExpress> <TSP>* "-" <TSP>* <MultiplicativeExpress>}
<ShiftExpression>       ::= {<AdditiveExpress>}
                            {<ShiftExpression> <TSP>* "<<" <TSP>* <AdditiveExpress>}
                            {<ShiftExpression> <TSP>* ">>" <TSP>* <AdditiveExpress>}
<RelationalExpress>     ::= {<ShiftExpression>}
                            {<RelationalExpress>} <TSP>* "<" <TSP>* <ShiftExpress>}
                            {<RelationalExpress>} <TSP>* "LT" <TSP>* <ShiftExpress>}
                            {<RelationalExpress>} <TSP>* ">" <TSP>* <ShiftExpress>}
                            {<RelationalExpress>} <TSP>* "GT" <TSP>* <ShiftExpress>}
                            {<RelationalExpress>} <TSP>* "<=" <TSP>* <ShiftExpress>}
                            {<RelationalExpress>} <TSP>* "LE" <TSP>* <ShiftExpress>}
                            {<RelationalExpress>} <TSP>* ">=" <TSP>* <ShiftExpress>}
                            {<RelationalExpress>} <TSP>* "GE" <TSP>* <ShiftExpress>}
<EqualityExpression>    ::= {<RelationalExpress>}
                            {<EqualityExpression> <TSP>* "==" <TSP>* <RelationalExpress>}
                            {<EqualityExpression> <TSP>* "EQ" <TSP>* <RelationalExpress>}
                            {<EqualityExpression> <TSP>* "!=" <TSP>* <RelationalExpress>}
                            {<EqualityExpression> <TSP>* "NE" <TSP>* <RelationalExpress>}
<BitwiseAndExpression>  ::= {<EqualityExpression>}
                            {<BitwiseAndExpression> <TSP>* "&" <TSP>* <EqualityExpression>}
<BitwiseXorExpress>     ::= {<BitwiseAndExpression>}
                            {<BitwiseXorExpress> <TSP>* "^" <TSP>* <BitwiseAndExpression>}
<BitwiseOrExpress>      ::= {<BitwiseXorExpress>}
                            {"(" <BitwiseOrExpress> <TSP>* "|" <TSP>* <BitwiseXorExpress> ")"}
<LogicalAndExpress>     ::= {<BitwiseOrExpress>}
                            {<LogicalAndExpress> <TSP>* "&&" <TSP>* <BitwiseOrExpress>}
                            {<LogicalAndExpress> <TSP>* "AND" <TSP>* <BitwiseOrExpress>}
                            {<LogicalAndExpress> <TSP>* "and" <TSP>* <BitwiseOrExpress>}
<LogicalXorExpress>     ::= {<LogicalAndExpress>}
                            {<LogicalAndExpress> <TSP>* "XOR" <TSP>* <LogicalXorExpress>}
                            {<LogicalAndExpress> <TSP>* "xor" <TSP>* <LogicalXorExpress>}
<LogicalOrExpress>      ::= {<LogicalXorExpress>}
                            {"(" <LogicalXorExpress> <TSP>* "||" <TSP>* <LogicalOrExpress> ")"}
                            {<LogicalXorExpress> <TSP>* "OR" <TSP>* <LogicalOrExpress>}
                            {<LogicalXorExpress> <TSP>* "or" <TSP>* <LogicalOrExpress>}
<CondExpress>           ::= {<LogicalOrExpress>}
                            {<LogicalOrExpress> <TSP>* "?" <IsTrue> ":" <TSP>* <CondExpress>
<IsTrue>                ::= <TSP>* <Expression> <TSP>*
<Expression>            ::= {<CondExpress>} {<Expression>}
```

#### Example

```ini
gUefiCpuPkgTokenSpaceGuid.PcdCpuLocalApicBaseAddress | !gCrownBayTokenSpaceGuid.PcdProgrammableLocalApic
```

#### Notes

**MACROVAL**

This is the value of the MACRO assigned in a DEFINE statement.

**Expressions**

If the "|" character is used in an expression, the expression must be
encapsulated by parenthesis.

## 3.2 Conditional Directive Expressions

Conditional directive statements are defined in the EDK II Platform Description
(DSC) File and Flash Definition (FDF) File. The following EBNF describes the
format for expressions used in conditional directives. The format is based on
section 6.10 of the ANSI C-99 Specification.

#### Restrictions

**ConstantExpression**

The ConstantExpression in the !if statement must evaluate to either True (1) or
False (0).

#### Prototype

```c
<EOL>                ::= 0x0D 0x0A
<TSP>                ::= {0x20} {0x09}
<Group>              ::= {<GroupPart>} {<Group> <GroupPart>}
<GroupPart>          ::= {<IfSection>} {<TextLine>}
<IfSection>          ::= <IfGroup> [<ElifGroups>] [<ElseGroup>] <EndIfLine>
<IfGroup>            ::= {<TSP>* "!if" <TSP>+ <ConstantExpression> <EOL> [<Group>]
                         {<TSP>* "!ifdef" <TSP>+ <MACROorMACROVAL> <EOL> [<Group>]
                         {<TSP>* "!ifndef" <TSP>+ <MACROorMACROVAL> <EOL> [<Group>]
<MACROorMACROVAL>    ::= {<MACRO>} {"$(" <MACRO> ")"}
<ElifGroup>          ::= {<ElifGroup>} {<ElifGroups> <ElifGroup>}
<ElifGroup>          ::= <TSP>* "!elif" <TSP>* <ConstantExpression> <EOL> [<Group>]
<ElseGroup>          ::= <TSP>* "!else" <TSP>* <EOL> [<Group>]
<EndIfLine>          ::= <TSP>* "!endif" <EOL>
<TextLine>           ::= Content specific to the meta-data file
<ConstantExpression> ::= <CondExpress> ; see Data Field Expression definitions
```

#### Example

```ini
!if $(LOGGING)
  DebugLib|IntelFrameworkModulePkg/Library/PeiDxeDebugLibReportStatusCode/PeiDxeDebugLibReportStatusCode.inf
  DebugPrintErrorLevelLib|MdePkg/Library/BaseDebugPrintErrorLevelLib/BaseDebugPrintErrorLevelLib.inf
!else
  DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
!endif

!if $(LOGGING) == FALSE
  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x0
  gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask|0x3
!endif
```
