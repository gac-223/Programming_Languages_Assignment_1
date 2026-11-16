
1. The original python interpreter seems to conform to all specifications. However, after deeper analysis, it is shown that the interpreter currently only evaluates outer expressions and does not evaluate inner expressions. We expect everything to get reduced to a normal form, and the original interpreter does not do that.

2. a b c d reduces to (((a b ) c ) d) due to left associativity while (a) reduces to a because (a) is just the variable a.

3. In this interpreter, capture avoiding substitution works by enforcing renaming of variables every time. Every variable is renamed to VarN where N is an atomic counter that increments for each rename. Furthermore because the grammar enforces lowercase, having and uppercase V in Var ensures that every rename VarN will be unique

4. In the original interpreter, you don't always get the expected result. Not all results reduce to normal form. For example, (\x.(\y.y) x) reduces to (\x.((\y.y) x)) instead of to (\x.x). After exploring the interpreter, we can see that we are only actually "reducing" the very first step, and everything else is left unreduced. Furthermore, any rercursive lambda expressions would cause errors in our program, so that would also I guess be unexpected.

5. (\x.(\y.y) x)
