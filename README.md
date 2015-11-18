# Compiler-design-BASCOM-
Developed a lexical analyser (compiler) on unix


Introduction

We wish to design a compiler for the hardware programming language BASCOM. The compiler would take as input, a BASCOM program with the file extension ‘.bas’ and compile the program making it more efficient for execution, optimizing the code. The compiler would also check for any errors in the grammar of the program as well other syntactical errors. The compiler can also check for data-type errors, since it would do a type analysis for every calculation or comparison which it detects.
 

Abstract

Each line in a BASCOM program begins with a unique line number followed by a keyword which marks the beginning of an instruction. Some of the keywords which also define the operations that are performed in BASCOM are:
•	Config: Config command assigns a port as an input or output port
eg: 10 Config Portd = Output
   	      20 Config Porta.3 = Input

•	Lcd: displays a message on the lcd or other output device.
eg: 10 Lcd Value
      20  Lcd "x"

•	Dim: Dim command is used to declare variables in the beginning of the program 
	eg: 10 Dim Count As Integer

•	Getadc(): asks the user to enter the value of a variable. The statement may include a prompt message.
eg: 30 Value = Getadc(0)

•	If ... Then ... Else.. Endif: used to perform comparisons or make decisions.
eg:  50 if x > 0 then goto 100 else goto 80

•	For … To: repeat a section of code a given number of times. A variable that acts as a counter is available within the loop.
		eg: 20 for i = 1 to num
		      30 for j = i to num
		      40 print i;" ";j
      50 next j:next i

•	Goto: Jumps to a numbered or labeled line in the program.
	eg: 50 goto 100

Gosub: Jumps to a subroutine given by the line number.Returns back to the next line on encountering the RETURN statement.
	Eg: 60 gosub 120

Variable names can be 32 characters long. The characters can be either letters or numbers, but the first character must be a letter. 
Bascom recognizes the following types of variables:
	Bit
Holds either the value 0 or 1. Use a bit variable to store i.e. the state of a button, switch or input/output pin, anything that is either on or off.
	Byte
An eight-bit byte can store an integer value between 0 and 255.
	Word
This is an unsigned two-byte integer variable. All sixteen bits are available, so the range of possible values will be 0 – 65535.
	String
A string variable can hold a sequence of characters.



 
Grammar
Instr ->Lno Command
Command -> Keyword Options
Options ->ε | Option Op1
Op1 -> : Command | ε
Option -> Variable Opt | Num | Str 
Opt ->ε | Op Varval
Varval -> Num Expr Expr1 | Variable Expr Expr1
Expr ->ε | Op Exp
Exp -> Num Expr | Variable Expr
Expr1 -> Keyword Key |  ε
Key -> Variable Expr | Keyword Option
Str ->ε | “String” Str1
Str1 ->ε | Punctuation Variable Str1 
Punctuation -> , | . | ε
Lno -> Digit Lno | ε
Variable -> Alpha Variable1
Variable1 ->Alpha Variable1 | Num Variable1 | ε
Alpha -> A-Z  Alpha | a-z  Alpha | ε
Num -> Digit Num | ε
Alphanumspec ->Alpha  | ! | @ | # | $ | % | ^ |& | ( | ) | { | } | [ | ] | ; | : | ‘ | , | . | < | > | / | ?
String ->Alphanumspec String | ε
Keyword -> if | then | goto | for | return |end | let | input | print | gosub |next | else 
Digit -> 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 0 
Op -> + | - | * | / 

 

Lexical Analyzer
The Lexical Analyzer has been implemented in C. For a given code (.bas file), it generates a text file that contains the Token name, its Type (keyword, identifier, constant, operator), its value type (integer, assignment, string),  Line number and Column Number and Token Number according to the type of token.
