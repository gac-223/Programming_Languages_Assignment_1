Lark Grammar Definitions

?start: exp: Defines the starting rule for parsing an expression. All input begins with exp.

?exp: exp "+" exp1 -> plus | exp "-" exp1 -> minus | exp1: Handles addition and subtraction. The -> symbols assign names to the operations so the transformer can process them.

?exp1: exp1 "*" exp2 -> times | exp2: Handles multiplication and gives it higher precedence than addition and subtraction.

?exp2: "-" exp2 -> neg | exp3: Handles unary negation, allowing numbers and expressions to be negative.

?exp3: "log" exp4 "base" exp4 -> log | exp4: Handles logarithms. -> symbol assigns name to log so that the transformer can process it.

?exp4: exp5 "^" exp4 -> power | exp5: Handles exponentiation. right associativity, so we want to recurse on the right operand and create groups

?exp5: NUMBER -> num | "(" exp ")": Handles number detection and parentheses. NUMBER is a Terminal and a Normal Form. If not a number, then it is some expression in parentheses that must be calculated

CalcTransformer Methods

plus(self, items): Returns ('plus', left, right) to represent addition and puts items[0] in the left child and items[1] in the right child of the "plus" subtree.

minus(self, items): Returns ('minus', left, right) to represent subtraction and puts items[0] in the left child and items[1] in the right child of the "minus" subtree.

times(self, items): Returns ('times', left, right) to represent multiplication and puts items[0] in the left child and items[1] in the right child of the "multiplication" subtree.

neg(self, items): Returns ('neg', operand) to represent unary negation.

log(self, items): Returns('log', left, right) to represent logarithms. Puts the argument in the left child and base in the right child of the "log" subtree

power(self, items): Returns ('power', left, right) to represent exponentiation. Puts the base in the left child and the exponent in the right child of the "power" subtree

num(self, items): Returns ('num', FLOAT) to represent the Decimal value of the number. Normal Form

Evaluate Function

The evaluate(ast) function recursively calculates the result of the AST. It checks the first tuple element to determine which operation to perform.

if ast[0] == 'plus': Evaluates both operands and adds them.

elif ast[0] == 'minus': Evaluates both operands and subtracts the second from the first.

elif ast[0] == 'times': Evaluates both operands and multiplies them.

elif ast[0] == 'neg': Evaluates the operand and multiplies it by -1.

elif ast[0] == 'log': Evaluates both operands and then uses Pythons math library to return log left base right. Left is the argument and Right child is the base

elif ast[0] == 'power': Evaluates both operands and then returns left ^ (right)

elif ast[0] == 'num': Returns the float number (Normal Form) 

Main Program Flow

The program reads the grammar from grammar.lark and initializes the Lark parser. Then
creates a CalcTransformer object, then reads the expression from the command-line argument sys.argv[1].
Next it parses the expression into a concrete syntax tree, transforms the tree into an AST, calls 
evaluate(ast) to calculate the result, and finally prints out the final result.

Decisions about CFG and Order of Operations

We decided to simply follow the order of operations as we know them. PEMDAS. Using the shape of the tree, the operations with the highest precedence are evaluated first. NUMBER is the normal form in this syntax/grammar.

Operations ordered by precedence increasing
exp
plus, minus
times
neg
log
power
num, "(" exp ")"

Considering associativity, left associativity means evaluate left to right. This means in the grammar, we want to recurse on the left operand in order to evaluate the left operand first. Right associativity means evaluate right to left, and so we recurse on the right operand in order to evaluate the right operand first.

plus and minus being left associativity ensures a - b - c is correctly parsed as (a - b) - c and not as a - (b - c)

power being right associative ensures a^b^c is correctly parsed as a^(b^c) and not as (a^b)^c


The testing.py file was copied from the provided template along with testing-data.txt