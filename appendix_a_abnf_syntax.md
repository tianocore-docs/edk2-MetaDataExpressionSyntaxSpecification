<!--- @file
  Appendix A ABNF Syntax

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

# Appendix A ABNF Syntax

This section provides the Augmented Backus Naur Form of the Expression Format.

## A.1 Data Field Expression ABNF

```c
PrimaryExpression     ::= Identifier / Constant / StringLiteral / "(" Expression ")" / Function
Identifier            ::= CName / MacroValue
MacroValue            ::= "$(" MACRO ")"
MACRO                 ::= (A-Z) *(A-Z0-9_)
Function              ::= CName "(" Argument *(CSP Argument) ")"
Argument              ::= PrimaryExpression
CName                 ::= (a-zA-Z_) *(a-zA-Z0-9_)
Constant              ::= TrueFalse / Number / GuidValue / Array
TrueFalse             ::= True / False
True                  ::= "TRUE" / "True" / "true"
False                 ::= "FALSE" / "False" / "false"
Number                ::= Integer / HexNumber
Integer               ::= Base10
Base10                ::= (0-9) / ((1-9) *(0-9))
HexNumber             ::= Base16
Base16                ::= HexPrefix *(HexDigit) HexDigit
HexDigit              ::= (a-fA-F0-9)
HexPrefix             ::= "0x" / "0X"
GuidValue             ::= RformatGuid / CformatGuid
Rhex2                 ::= [HexDigit] HexDigit
Rhex4                 ::= [HexDigit] [HexDigit] Rhex2
Rhex8                 ::= [HexDigit] [HexDigit] [HexDigit] [HexDigit] Rhex4
Rghex2                ::= HexDigit HexDigit
Rghex4                ::= HexDigit HexDigit Rghex2
Rghex8                ::= HexDigit HexDigit HexDigit HexDigit Rghex4
Rghex12               ::= HexDigit HexDigit HexDigit HexDigit Rghex8
RformatGuid           ::= Rghex8 "-" Rghex4 "-" Rghex4 "-" Rghex4 "-" Rghex12
Byte                  ::= HexPrefix Rhex2
Hex16                 ::= HexPrefix Rhex4
Hex32                 ::= HexPrefix Rhex8
CSP                   ::= %x2c *(TSP)
TSP                   ::= %x20 / %x09
CformatGuid           ::= "{" *(TSP) Hex32 CSP Hex16 CSP Hex16 CSP Part2
Part2                 ::= "{" *(TSP) Byte CSP Byte CSP Byte CSP Byte CSP Part3
Part3                 ::= Byte CSP Byte CSP Byte CSP Byte *(TSP) "}" *(TSP) "}"
Array                 ::= EmptyArray / Array
EmptyArray            ::= "{" *(TSP) "}"
ByteArray             ::= "{" *(TSP) Byte *(CSP Byte) "}"
StringLiteral         ::= QuotedString / "L" QuotedString
DblQuote              ::= %x22
QuotedString          ::= DblQuote *(CCHAR) DblQuote
CCHAR                 ::= SingleChars / EscapeCharSeq
SingleChars           ::= %x20 / %x21 / %x23-5B / %x5D-7E
EscapeCharSeq         ::= "\" ("n" / "r" / "t" / "\" / "f" / "b" / "0" / DblQuote)
PostFixExpression     ::= PrimaryExpression / PcdName
PcdName               ::= CName "." CName
UnaryExpression       ::= PostFixExpression / UnaryOp UnaryExpression
UnaryOp               ::= IntegerOps / ScalarOps
IntegerOps            ::= "+" / "-" / "~"
ScalarOps             ::= "NOT" / "not" / "!"
MultiplicativeExpress ::= UnaryExpression /
                          ( MultiplicativeExpress *(TSP) "*" *(TSP) UnaryExpression ) /
                          ( MultiplicativeExpress *(TSP) "/" *(TSP) UnaryExpression ) /
                          ( MultiplicativeExpress *(TSP) "%" *(TSP) UnaryExpression )
AdditiveExpress       ::= MultiplicativeExpress /
                          ( AdditiveExpress *(TSP) "+" *(TSP) MultiplicativeExpress ) /
                          ( AdditiveExpress *(TSP) "-" *(TSP) MultiplicativeExpress )
ShiftExpression       ::= AdditiveExpress /
                          ( ShiftExpression *(TSP) "<<" *(TSP) AdditiveExpress )
                          ( ShiftExpression *(TSP) ">>" *(TSP) AdditiveExpress )
RelationalExpress     ::= ShiftExpression /
                          ( RelationalExpress *(TSP) "<" *(TSP) ShiftExpress ) /
                          ( RelationalExpress *(TSP) ">" *(TSP) ShiftExpress ) /
                          ( RelationalExpress *(TSP) "<=" *(TSP) ShiftExpress ) /
                          ( RelationalExpress *(TSP) ">=" *(TSP) ShiftExpress)
EqualityExpression    ::= RelationalExpress /
                          ( EqualityExpression *(TSP) "==" *(TSP) RelationalExpress ) /
                          ( EqualityExpression *(TSP) "EQ" *(TSP) RelationalExpress ) /
                          ( EqualityExpression *(TSP) "!=" *(TSP) RelationalExpress ) /
                          ( EqualityExpression *(TSP) "NE" *(TSP) RelationalExpress ) /
BitwiseAndExpression  ::= EqualityExpression /
                          ( BitwiseAndExpression *(TSP) "&" *(TSP) EqualityExpression )
BitwiseXorExpress     ::= BitwiseAndExpression /
                          ( BitwiseXorExpress *(TSP) "^" *(TSP) BitwiseAndExpression )
BitwiseOrExpress      ::= BitwiseXorExpress /
                          ( "(" BitwiseOrExpress *(TSP) "|" *(TSP) BitwiseXorExpress ")" )
LogicalAndExpress     ::= BitwiseOrExpress /
                          ( LogicalAndExpress *(TSP) "&&" *(TSP) BitwiseOrExpress ) /
                          ( LogicalAndExpress *(TSP) "AND" *(TSP) BitwiseOrExpress /
                          ( LogicalAndExpress *(TSP) "and" *(TSP) BitwiseOrExpress )
LogicalXorExpress     ::= LogicalAndExpress /
                          ( LogicalAndExpress *(TSP) "XOR" *(TSP) LogicalXorExpress ) /
                          ( LogicalAndExpress *(TSP) "xor" *(TSP) LogicalXorExpress )
LogicalOrExpress      ::= LogicalXorExpress /
                          ( "(" LogicalXorExpress *(TSP) "||" *(TSP) LogicalOrExpress ")" ) /
                          ( LogicalXorExpress *(TSP) "OR" *(TSP) LogicalOrExpress ) /
                          ( LogicalXorExpress *(TSP) "or" *(TSP) LogicalOrExpress )
CondExpress           ::= LogicalOrExpress /
                          ( LogicalOrExpress *(TSP) "?" IsTrue ":" *(TSP) CondExpress )
IsTrue                ::= *(TSP) Expression *(TSP)
Expression            ::= CondExpress / Expression
```

## A.2 Conditional Directive Expression ABNF

```c
EOL                ::= %x0D.0A
TSP                ::= %x20 / %x09
Group              ::= GroupPart / ( Group GroupPart )
GroupPart          ::= IfSection / TextLine
IfSection          ::= IfGroup [ElifGroups] [ElseGroup] EndIfLine
IfGroup            ::= ( *(TSP) "!if" 1*(TSP) ConstantExpression EOL [Group] ) /
                       ( *(TSP) "!ifdef" 1*(TSP) MACROorMACVAL EOL [Group] ) /
                       ( *(TSP) "!ifndef" 1*(TSP) MACROorMACVAL EOL [Group]
MACROorMACVAL      ::= MACRO / MacroValue
ElifGroups         ::= ElifGroup / ( ElifGroups ElifGroup )
ElifGroup          ::= *(TSP) "!elif" 1*(TSP) ConstantExpression EOL [Group]
ElseGroup          ::= *(TSP) "!else" *(TSP) EOL [Group]
EndIfLine          ::= *(TSP) "!endif" EOL
TextLine           ::= Content specific to the meta-data file
ConstantExpression ::= CondExpress ; see Data Field Expression definitions
```
