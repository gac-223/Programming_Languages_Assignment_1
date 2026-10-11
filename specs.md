Lark Grammar Definitions

?start: exp: Defines the starting rule for parsing an expression. All input begins with exp.

?exp: exp "+" exp1 -> plus | exp "-" exp1 -> minus | exp1: Handles addition and subtraction. The -> symbols assign names to the operations so the transformer can process them.

?exp1: exp1 "*" exp2 -> times | exp2: Handles multiplication and gives it higher precedence than addition and subtraction.

?exp2: "-" exp2 -> neg | exp3: Handles unary negation, allowing numbers and expressions to be negative.

CalcTransformer Methods

plus(self, items): Returns ('plus', left, right) to represent addition and puts items[0] in the left child and items[1] in the right child of the "plus" subtree.

minus(self, items): Returns ('minus', left, right) to represent subtraction and puts items[0] in the left child and items[1] in the right child of the "minus" subtree.

times(self, items): Returns ('times', left, right) to represent multiplication and puts items[0] in the left child and items[1] in the right child of the "multiplication" subtree.

neg(self, items): Returns ('neg', operand) to represent unary negation.

Evaluate Function

The evaluate(ast) function recursively calculates the result of the AST. It checks the first tuple element to determine which operation to perform.

if ast[0] == 'plus': Evaluates both operands and adds them.

elif ast[0] == 'minus': Evaluates both operands and subtracts the second from the first.

elif ast[0] == 'times': Evaluates both operands and multiplies them.

elif ast[0] == 'neg': Evaluates the operand and multiplies it by -1.

Main Program Flow

The program reads the grammar from grammar.lark and initializes the Lark parser. Then
creates a CalcTransformer object, then reads the expression from the command-line argument sys.argv[1].
Next it parses the expression into a concrete syntax tree, transforms the tree into an AST, calls 
evaluate(ast) to calculate the result, and finally prints out the final result.