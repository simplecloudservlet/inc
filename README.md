
# INC (INC 'I's 'N'ot a 'C'ompiler) v1.0

 Author: Lucio A. Rocha
 Last update: 06-09-2024

---
## 1. Description:
An educational LL(1) parser.

## 2. Quick start: 
---
#### 2.1 Create a 'file.txt' (more examples in the 'languages' folder), e.g.:

> S
> %
> S
> A
> B
> C
> D
> %
> 0
> 1
> %
> \#
> %
> S->A
> A->0A1
> A->B
> B->C
> C->1C
> C->#

#### 2.2 Run the INC, e.g.:
>$ java -jar INC_app-1.0.jar file.txt 

#### 2.3 Try some sentences, e.g.:
> \>\> 0011

#### 2.4 Insert '!q' or 'Ctrl+C' to quit.

---
# Detailed start:

### 3. How to run:
---
> $ java -jar INC_app-[VERSION].jar <FILE.txt> [SHOW_DEBUG:true|false|-t]

### 4. How to run and save the log in file:
---
> $ java -jar INC_app-[VERSION].jar <FILE.txt>  &> log.txt

### 5. Important notes:
---
#### 5.1. You should to replace rules with left recursion, e.g.:

 >A->A+B

with these new rules:

 >A->+BX
 X->+BX
 X-># (# is the empty symbol here)

#### 5.2. You can not have two rules with the same first symbol in RHS (Right-hand side) for some recursive expansion, e.g.:

 >A->a
 A->a(A)

---
## 6. How to write grammars:
---

Look at the folder 'languages' to get some examples.

---
#### 6.1 <FILE.txt> format:
---
> \<SENTENCIAL SYMBOL>
%
\<NON_TERMINAL SYMBOLS>
%
\<TERMINAL SYMBOLS>
%
\<EMPTY SYMBOL>
%
\<RULES>
-------------------------

---
#### 6.2 Example of <FILE.txt>: Language: L={0^n 1^m0 1^n | n>0, m>=0}
---
>S
%
S
A
B
C
%
0
1
%
\#
%
S->A
A->0A1
A->B
B->C
C->1C
C->#

---
Expected output:

> \[TABLE]:first
1:A:[0, 1]
2:B:[1, 0]
3:C:[1, 0]
4:S:[0, 1]
[TABLE]:follow
1:A:[1, $]
2:B:[1, $]
3:S:[$]
4:C:[1, $]
[TABLE]:rulesl1
1:A:0A1
2:A:B
3:B:C
4:S:A
5:C:1C
6:C:0
[TABLE]:LL(1)
 	0	1	$	
S	4	4	-	
A	1	2	-	
B	3	3	-	
C	6	5	-	
D	-	-	-
---

#### 6.3 Example of <FILE.txt>: Language: a, a+a, (a*a), (a+(a*a)), ...
---
> P
%
P
A
B
C
D
E
%
\+
(
)
a
\*
%
\#
%
P->A
A->BC
C->+BC
C->#
B->DE
E->*DE
E->#
D->(A)
D->a


---
Expected output:
> \[TABLE]:first
1:P:[(, a]
2:A:[(, a]
3:B:[(, a]
4:C:[+, #]
5:D:[(, a]
6:E:[*, #]
[TABLE]:follow
1:P:[\$]
2:A:[\$, )]
3:B:[+, \$, )]
4:C:[\$, )]
5:D:[*, +, \$, )]
6:E:[+, \$, )]
[TABLE]:rulesl1
1:P:A
2:A:BC
3:B:DE
4:C:+BC
5:C:#
6:D:(A)
7:D:a
8:E:*DE
9:E:#
[TABLE]:LL(1)
 	+	(	)	a	*	$	
P	-	1	-	1	-	-	
A	-	2	-	2	-	-	
B	-	3	-	3	-	-	
C	4	-	5	-	-	5	
D	-	6	-	7	-	-	
E	9	-	9	-	8	9

---
## 7. (Opcional) How to compile and run (with Maven):
-----------------------------------------------
> $ mvn clean install
$ java -jar target/INC_app-[VERSION].jar 

---
## 8. (Opcional) How to compile, pack and run (from source):
---
> $ javac -cp . INC.java
$ jar cvf INC.jar *.class
$ java  -cp . INC <FILE.txt>
$ java -jar INC.jar <FILE.txt>

---
End of file.
