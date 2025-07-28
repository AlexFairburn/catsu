# Running
- Just use
    `./catsu <FILENAME>`

# Syntax
Catsu's syntax is based on two-letter mnemonics. 

## Variables
### Data types
Currently, Catsu supports the following data types:
- int (Integer)
- float (Real)
- bool (Boolean)
- str (String)

You can cast using the functions `toInt()`, `toFloat()` and `toString()`.

### Expressions
Expressions in Catsu use Polish notation/Forwards Polish notation.
Essentially, the operands come after the operator.

The two operands in an expression must be of the same type.

#### Arithmetic operations
These are the arithmetic operations currently supported:

- Add `+ 4 2`
- Subtract `- 13 3`
- Multiply `* 15 2`
- Divide `/ 15 3`
- Exponent `^ 2 3`
- Modulus `% 16 2`

#### Logical operations
Yes logic

- Equality `== a b`
- Inequality `!= a b`
- An actual inequality `< a b`
- Another! `> a b`
- Some more `<= a b`
- I'm feeling generous `>= a b`
- Logical AND `/AND a b`
- Logical OR `/OR a b`
- Nand `/NAND a b`
- Nor `/NOR a b`
- Exclusive or `/XOR a b`

#### String operations

- Concatenation `+ "My name is: " "Brian"`
- Fancy concatenation (It adds a space!) `, "My name is:" "Brian"`
- String index `[0]."my string"`


#### Array operations
- Append two arrays `+ [1,2,3] [4,5,6]`
- Append a single element `+ [1,2,3] 4`
- Prepend an array `: [4,5,6] [1,2,3]`
- Prepend an item `: [1,2,3] 0`


### Defining a variable
To define a variable, use ```dv <name>:<type> = <expression>```
For example:
``` 
    dv x:int = 12;
    dv name:str = "Terrence";
    dv isCool:bool = False;
    dv pi:float = 3.14;
```

>No spaces are allowed between the variable's name and its type.
>To discourage snake case, variable names can only be made up of letters.

Variables can be initialised without a starting value.
`dv i:int;` is equivalent to `dv i:int = 0;`.


Arrays are also supported:

```
    dv names:[str] = ["Euler", "Tao", "Gauss", "Godel"];
```
Due to my brilliance, empty arrays cannot exist. For an
"empty" array, use

```
    dv grades:[int] = [0];
```
Arrays start at 0 and can be accessed using `[<index>].<array>`
```
     dv names:[str] = ["Wilson", "Cuddy", "House"];
    dv theGoat:str = [0].names;
```

### Modifying a variable
One does not simply redefine a variable in order to modify it.
In Catsu, one uses a specific statement.

In general, `mv <name> = <expression>`
 
```
    dv age:int = 56;
    mv age = 57;
```
When modifying arrays, you can reference specific indices.

```
    dv grades:[int] = [89, 70, 0, 12, 34];
    mv [2].grades = 100;
```
The same rationale applies for strings.


## If/Else
```
    if <expression>
    {
        execute this code
    }
    el
    {
        otherwise do this
    }

    dv age:int = 99;
    if (> age 85)
    {
        dv isOld:bool = True;
    }
    el
    {
        dv isOld:bool = False;
    }

    Alternatively,
    dv age:int = 99;
    dv isOld:bool = False;
    if (> age 89) {mv isOld = True;}

```
If statements do not have to be accompanied by else.
The conditions don't have to be in brackets.

## Iteration
### For loops
```
    fo {run this first}(condition){run this at the end of each block}
    {
        main body here
    }
```
Note that multiple statements are allowed in each of the sets of braces.
You could in theory leave the final set of braces empty and place the loop
body in the penultimate set.

```
    fo {dv i:int;}(< i 10){mv i = + 1 i;}{
        pr i;
    }
```

### While loops

```
    wl <condition>
    {
        run this
    }

    dv i:int;
    wl (< i 10){
        pr i;
        mv i = + i 1;
    }
```

## Functions
Functions are cool.

### Defining a function
```
    df <name>(<arg name>:<arg type>)(return:<return type>)
    {
        function body
    }
```
As Catsu is a statement based language, returning a value from a function
is slightly unorthodox. Return is actually a variable, so `rt <value>` is
actually syntactic sugar for `mv return = <value>`.

```
    df addTwo(a:int)(b:int)(return:int)
    {
        rt + a b;
    }
```
As the return value can be written to, you can do interesting things.

```
    df total(nums:[int])(return:int)
    {
        dv sum:int;
        fo {dv i:int;}(< i size(nums)){mv i = + i 1;}
        {
            mv sum = + sum [i].nums;
        }

        rt sum;
    }

    df coolTotal(nums:[int])(return:int)
    {
        fo {dv i:int;}(< i size(nums)){mv i = + i 1;}
        {
            mv return = + return [i].nums;        
        }
    }
```

`size()` is a built-in function by the way.

### Calling a function

```
dv sum:int = addTwo(2)(5);
dv sales:[int] = [5,7,1,12,900];
dv total:int = coolTotal(sales);
```

## Lambdas

### Lambda Definition

```
    dv x:lam = \(x:int) -> (^ 2 x);
```
I might've lied. Lambdas are a data type too. 

### Lambda application
Lambdas are applied like functions except with a backslash.
```
    dv y:int = \x(3);
```
Unfortunately, you can't do some of the fun higher-order functions things
as the scopes for lambdas need some work.

## Print Statements

```
    pr "Hello, world!";
    dv age:int = 99;
    pr , "age:" age;
```

## Subroutines
Inspired by Applesoft basic. This is a way of repeating code that can
affect the outer state of the program.
To work with subroutines, use "define sub" and "gosub" statements.

```
    dv i:int = 12;

    ds incrementAndOutput
    {
    mv i = + i 1;
    pr i;
    }
    
    gs incrementAndOutput;
```
