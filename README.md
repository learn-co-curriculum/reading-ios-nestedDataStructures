# Nested Data Structures

## Objectives

1. Recognize the value of nesting.
2. Read and write nested method calls.
3. Create, access, and edit (mutable only) nested arrays.
4. Create, access, and edit (mutable only) nested dictionaries.
5. Recognize a nested combination and interpret how to access it.

## Introduction

If you've ever handled a set of Matryoshka dolls then you've been introduced to the concept of nesting: placing a thing inside of another thing.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-nested-data/robot-nesting-dolls.png)

*Robot Nesting Dolls,* available online.

In programming, the term "nesting" can refer to either placing method calls within other methods, or to placing collection objects (e.g. `NSArray` or `NSDictionary` objects) within other collection objects.

## Nested Method Calls

Whether you realized it or not, you've already used nested method calls. The most common example of a nested method call is the pairing of `+alloc` with an `-init` method on the same line:

```objective_c
NSMutableArray *myArray = [[NSMutableArray alloc] init];
```
What this means implicitly is for the program to take the result of `[NSMutableArray alloc]` class method and make it the recipient object of the `init` instance method. In effect, we're capturing the return of `[NSMutableArray alloc]` directly into another method call. It's equally valid (though unusual) to write this on two lines:

```objective_c
NSMutableArray *myArray = [NSMutableArray alloc];
myArray = [myArray init];
```
Nesting a method call is useful in almost any case that you only need to interact with its result once, such as altering it before adding it to a collection:

```objective_c
NSString *whaleception = @"whaleception";

[myArray addObject:[whaleception uppercaseString]];

NSLog(@"%@", myArray);
```
Which is equivalent to:

```objective_c
NSString *whaleception = @"whaleception";

NSString *whaleceptionUC = [whaleception uppercaseString];
[myArray addObject:whaleceptionUC];

NSLog(@"%@", myArray);
```

Either will print:

```
(
    WHALECEPTION
)
```
Or altering an object in order to perform a check on it:

```objective_c
if ([ [myArray[0] lowercaseString] isEqualToString:whaleception]) {
    NSLog(@"A whale within a whale.");
}
```
Which is equivalent to:

```objective_c
NSString *whaleceptionLC = [myArray[0] lowercaseString];
if ([whaleceptionLC isEqualToString:whaleception]) {
    NSLog(@"A whale within a whale.");
}
```
Either will print: `A whale within a whale.`.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-nested-data/4559-whaleception.jpg)

The primary usefulness of method nesting is to reduce the line count in your code and to improve readability. As a rule of thumb, nesting methods more than two or three levels deep begins to go against this purpose. Particularly when the brackets begin to pile up at the end of a statement, readability starts to diminish. To improve readability and to help you keep them in order, either use whitespace to group sets of brackets into pairs or threes, or just break out your nesting into separate statements.

## Nested Collections

The other form of nesting in programming is that of collection objects. Let's start by looking at what we can do by nesting arrays.

### Matrices: Nested Arrays

Nesting arrays is how a computer can understand a two-dimensional matrix, such as a tic-tac-toe board. We can create an "array of arrays" pretty simply using the array literal syntax like this:

```objective_c
NSArray *ticTac = @[ @[ @"1", @"2", @"3" ]   ,
                     @[ @"4", @"5", @"6" ]   ,
                     @[ @"7", @"8", @"9" ]  ];
```
We've just created a three-by-three square! For readability, we've written this single statement on three separate lines, but the compiler will translate it just the same as if we had written it on a single line:

```objective_c
NSArray *ticTac = @[ @[ @"1", @"2", @"3" ], @[ @"4", @"5", @"6" ], @[ @"7", @"8", @"9" ] ];
```
So, why didn't we just a write a single array of nine strings? Organization. Each sub-array in `ticTac` represents a different row. That means the sub-arrays' indexes line up into columns. So,  now that we have an array of arrays, the sub-arrays don't have their own names. How do we access just a single sub-array? Through the super-array (or "parent array"), of course!

#### Accessing A Sub-array

You should recall the array accessor literal that accepts a valid index:

```objective_c
Class *element = array[index];
```
But this literal syntax also allows us to access a sub-array by index by chaining the literals using this format:

```objective_c
Class *element = array[index][sub-index];
```
While we could access the sub-array in two steps by first capturing the return of an index call on `ticTac`:

```objective_c
NSArray *row0 = ticTac[0];
NSString *tileThree = row0[2];

NSLog(@"%@", tileThree);
```
This will print: `3`.

The literal syntax allows us access a sub-array within `ticTac` without having to name the sub-array in a captured return:

```objective_c
NSString *tileThree = ticTac[0][2];

NSLog(@"%@", tileThree);
```
This will also print: `3`.

So, we're able to use the literal syntax to access the same sub-array without having to give it an individual name. We can actually chain this literal syntax as deep as the sub-arrays go. For example, if we have a three-dimensional tic-tac-toe cube initialized like so:

```objective_c
NSArray *ticTac3D = @[ @[ @[ @"1", @"2", @"3" ]  ,
                          @[ @"4", @"5", @"6" ]  ,
                          @[ @"7", @"8", @"9" ]  ]  ,
                     
                       @[ @[ @"a", @"b", @"c" ]  ,
                          @[ @"d", @"e", @"f" ]  ,
                          @[ @"g", @"h", @"i" ]  ]  ,
                     
                       @[ @[ @"j", @"k", @"l" ]  ,
                          @[ @"m", @"n", @"o" ]  ,
                          @[ @"p", @"q", @"r" ]  ] ];
```
We can access a sub-sub-array containing a string by chaining a third literal accessor:

```objective_c
NSString *cubieE = ticTac3D[1][1][1];
    
NSLog(@"%@", cubieE);
```
This will print: `e`.

#### Setting A Sub-mutable-array

If we try using the literal syntax to overwrite one of the objects in our 2-D `ticTac` board with the setter literal syntax in this manner:

```objective_c
ticTac[1][1] = @"x";
```
We'll get a crash. Remember, *the setter literal syntax only works for mutable types*. We'd have to declare the sub-arrays as mutable arrays in order to overwrite their contents:

```objective_c
NSArray *ticTac = @[ [@[ @"1", @"2", @"3" ] mutableCopy]  ,
                     [@[ @"4", @"5", @"6" ] mutableCopy]  ,
                     [@[ @"7", @"8", @"9" ] mutableCopy] ];
```
Now we can run our setter literal:

```objective_c
NSLog(@"Before setter: %@", ticTac[1][1]);
    
ticTac[1][1] = @"x";
    
NSLog(@"After setter: %@", ticTac[1][1]);
```
This will print:

```
Before setter: 5
After setter: x
```
Of all the objects in our current scope: our nine string objects, three sub-mutable-arrays, and one super-array; the only object we've actually named is the super-array `ticTac`. Everything else we're able to access through this nested collection.

### Nested Dictionaries

In a similar way to how arrays can be nested, dictionary collections can be nested as well. This takes the form of a sub-dictionary being set as the value to some key within a super-dictionary (or "parent dictionary). Let's take our contact list as an example:

```objective_c

NSDictionary *jennyCurran = @{ @"firstName"    : @"Jenny"            ,
                               @"lastName"     : @"Curran"           ,
                               @"relationship" : @"Friend"           ,
                               @"phoneNumber"  : @"(555) 867-5309"   ,
                               @"emailAddress" : @"jenny@email.com" };
    
NSDictionary *uncleBob = @{ @"firstName"    : @"Bob"                 ,
                            @"lastName"     : @""                    ,
                            @"relationship" : @"Uncle"               ,
                            @"phoneNumber"  : @"(555) 876-1234"      ,
                            @"emailAddress" : @"unclebob@email.com" };
```
We can save the individual dictionaries into a super-dictionary by passing them in as values with keys:

```objective_c
NSDictionary *contacts = @{ @"Jenny Curran" : jennyCurran  ,
                            @"Uncle Bob"    : uncleBob    };
```
This nested dictionary can also be generated without the intermediary step like so:

```objective_c
NSDictionary *contacts = @{ @"Jenny Curran" : @{ @"firstName"    : @"Jenny"               ,
                                                 @"lastName"     : @"Curran"              ,
                                                 @"relationship" : @"Friend"              ,
                                                 @"phoneNumber"  : @"(555) 867-5309"      ,
                                                 @"emailAddress" : @"jenny@email.com"     }  ,
                                
                            @"Uncle Bob"    : @{ @"firstName"    : @"Bob"                 ,
                                                 @"lastName"     : @""                    ,
                                                 @"relationship" : @"Uncle"               ,
                                                 @"phoneNumber"  : @"(555) 876-1234"      ,
                                                 @"emailAddress" : @"unclebob@email.com"  } };
```

#### Accessing A Nested Dictionary

And in the similar way that nested arrays can be accessed with the literal syntax, nested dictionaries can be accessed with the dictionary literal syntax:

```objective_c
Class *element = dictionary[key];
```
And for a sub-dictionary value:

```objective_c
Class *element = dictionary[key][sub-key];
```
Let's use this syntax to print Jenny's phone number:

```objective_c
NSString *jennysPhone = contacts[@"Jenny Curran"][@"phoneNumber"];

NSLog(@"%@", jennysPhone);
```
This will print: `(555) 867-5309`.

#### Setting A Nested Mutable Dictionary

The setter literal syntax for dictionaries also only works for mutable dictionaries. Attempting to set a key and value in a standard dictionary with the setter literal will cause a crash:

```objective_c
contacts[@"Uncle Bob"][@"lastName"] = @"Gump";
```

This will cause an error that reads like this:

```
*** Terminating app due to uncaught exception  
'NSInvalidArgumentException', reason: '-[__NSDictionaryI 
setObject:forKeyedSubscript:]: unrecognized selector sent to 
instance 0x7fd0e3eb6b60'
```
However, if we had initialized our sub-dictionaries as mutable dictionaries above, we'd be able to use the setter literal syntax. Let's change them with `mutableCopy`:

```objective_c
NSDictionary *contacts = @{ @"Jenny Curran" : [@{ @"firstName"    : @"Jenny"               ,
                                                  @"lastName"     : @"Curran"              ,
                                                  @"relationship" : @"Friend"              ,
                                                  @"phoneNumber"  : @"(555) 867-5309"      ,
                                                  @"emailAddress" : @"jenny@email.com"     }
                                               mutableCopy] ,
                                
                            @"Uncle Bob"    : [@{ @"firstName"    : @"Bob"                 ,
                                                  @"lastName"     : @""                    ,
                                                  @"relationship" : @"Uncle"               ,
                                                  @"phoneNumber"  : @"(555) 876-1234"      ,
                                                  @"emailAddress" : @"unclebob@email.com"  }
                                               mutableCopy] };
```
Now we can successfully correct Uncle Bob's last name:

```objective_c
contacts[@"Uncle Bob"][@"lastName"] = @"Gump";
    
NSLog(@"%@", contacts[@"Uncle Bob"][@"lastName"]);
```
This will print: `Gump`.

### Nested Combinations

Collections can be nested into collections of other types. While this isn't exclusive to arrays and dictionaries, the literal syntax available to these collections makes them the workhorses of managing large sets of data. Take, for example, and array of information on each of four adventuring hobbits:

```objective_c
NSArray *adventuringHobbits = @[ @{@"firstName" : @"Frodo"      ,
                                   @"lastName"  : @"Baggins"    ,
                                   @"nickname"  : @"Ringbearer" ,
                                   @"age"       : @50           ,
                                   @"inventory" : @[ @"ball of lint" 
                                                     @"Ring of Power" ]  
                                                                } ,
                                 @{@"firstName" : @"Samwise"    ,
                                   @"lastName"  : @"Gamgee"     ,
                                   @"nickname"  : @"Sam"        ,
                                   @"age"       : @38           } ,
                                 @{@"firstName" : @"Meriadoc"   ,
                                   @"lastName"  : @"Brandybuck" ,
                                   @"nickname"  : @"Merry"      ,
                                   @"age"       : @36           } ,
                                 @{@"firstName" : @"Peregrine"  ,
                                   @"lastName"  : @"Took"       ,
                                   @"nickname"  : @"Pippin"     ,
                                   @"age"       : @28           }
                                 ];
```
**A Note For Tolkien Fans:** *Character ages are those at the beginning of the "Lord of the Rings" trilogy and collected [here](http://tinw.hubpages.com/hub/lord-of-the-rings-characters-ages)* 

This is a nested combination: an array of dictionaries. Knowing the order the dictionaries in the array and their keys, we can chain the accessor literals of the array and the dictionary to go directly to the value of Pippin's age:

```objective_c
NSNumber *pippinsAge = adventuringHobbits[3][@"age"];

NSLog(@"Pippin is %@ years old.", pippinsAge);
```
This will print: `Pippin is 28 years old.`.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-nested-data/merryPippin.gif)

*Lets get another one...*

If you have keen eyes like Legolas, you may have noticed that Frodo has an extra dictionary key called `inventory` that points to an array with two objects in it. *What has it gots in its pocketses?* Well, we can access the contents of Frodo's inventory by chaining the accessor literals like this:

```objective_c
NSString *preciousss = adventuringHobbits[0][@"inventory"][1];

NSLog(@"Frodo has the %@!", preciousss);
```
This will print: `Frodo has the Ring of Power!`.

![](https://curriculum-content.s3.amazonaws.com/reading-ios-nested-data/frodoRing.gif)

Now take that thing to Mordor, Frodo!

<a href='https://learn.co/lessons/reading-ios-nestedDataStructures' data-visibility='hidden'>View this lesson on Learn.co</a>
