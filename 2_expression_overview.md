<!--- @file
  2 Expression Overview

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

# 2 Expression Overview

The expression syntax follows closely, the expression syntax and precedence of
C as defined in the ISO/IEC 9899:TC2 document. Since the EDK II meta-data files
are not parsed as C code, but rather as script files, some of the items listed
in the C expression section (6.5) do not apply. Also, some of the items have
been extended to use scripting syntax for logical comparison operators such as
"EQ" as a synonym for "==".

The Conditional directive section is loosely based on section 6.10,
Preprocessing directives. Again, some of the items listed in the Preprocessing
directives section do not apply.

## 2.1 Constraints and Semantics

1. Prior to evaluation, all macro values, PCD values and defined literals (such
   as TRUE or FALSE) used in the expression must be resolved. The only
   exception to this rule is when a macro value assignment contains an
   expression. In this case, an expression on the right side of the assignment
   operator ("=") must have all values resolved in the expression's operands
   prior to evaluation and subsequent assignment. In this instance, in order to
   have the operand values resolved, the tools may be required to perform more
   than one pass over a file to obtain values for the operands.

2. Floating point values are not supported under PCD datum types.

3. When used in an expression, a PCD's value is used during evaluation.

4. An expression is a sequence of operators and operands that specifies
   computation of a value.

5. Between the previous and next sequence, an object shall have its stored
   value modified at most once by the evaluation of an expression.

6. Some operators (the unary operator, ~, and the binary operators, `<<, >>`,
   &, | and ^ collectively described as bitwise operators) are required to have
   operands that are integer (base10) or hexadecimal (base16) types.

7. The unary arithmetic operators, +, - and ~ must have an operand that is an
   integer (base10) or hexadecimal (base16) type, while the scalar unary
   operators, ! and NOT, must have operands that are of type scalar.

8. The Boolean values, False, FALSE and false have a numerical value of zero.
   The Boolean values, True, TRUE and true have a numerical value of one.

9. Tools may be required to translate the expression sequence into an
   expression format that is comprehended by the tool's native language. For
   example, a tool written in Python may require changing the exact syntax of
   an expression to a syntax that can be processed by Python's eval() function.

10. Multiplicative and additive operators require both operands to be
    arithmetic in type.

11. Relational and equality operators require both operands to be of the same
    type. Relational comparison on string literals and byte arrays must be
    performed comparing the byte values, left to right. The first pair of bytes
    (or a single extra byte) that is not equal will determine the result of the
    comparison. The following are examples of string comparisons:
    ```
    Foo = "zero", Bar = "three";
    ```
    `Foo < Bar` will evaluate to zero (AKA, FALSE), as "z" is greater than "t".
    ```
    Foo = "thirty", Bar = "thirty1";`
    ```
    `Foo < Bar` will evaluate to one (AKA, TRUE), as Bar has an extra character.
    ```

12. Logical operators require operands that are type scalar.

13. For the Conditional Operator, the first operand must be scalar, while the
    second and third operands must have the same type (i.e., both being scalar,
    both being integers, or both being string literals).

14. Array format like "{0x10, 0x20}" can't be a operand of any operator except
    Relational and equality operators.
