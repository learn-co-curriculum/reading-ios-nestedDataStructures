#Nested Data Structures

As you start to look at other people's code, as well as get more comfortable with writing your own code, you will begin to encounter and write nested data structures. Take a few minutes to review examples of the following common nested method calls, `NSDictionary` objects, and `NSArray` objects.

##Methods

######Example
```objc
NSString *stringFromFloat = [[NSNumber numberWithFloat:23.0] stringValue];
```

These nested method calls first convert a `CGFloat` to an `NSNumber` and then convert the `NSNumber` in the outer method call to an `NSString`.

This could just have easily been written like this:

```objc
NSNumber *numberFromFloat = [NSNumber numberWithFloat:23.0];
NSString *stringFromNumber = [numberFromFloat stringValue];
```

Both of these are valid, and one is not "better" than the other. They are simply different ways of saying the same thing!

##Nested `NSDictionary` objects

######Example
```objc
NSDictionary *venue = {@"name":@"Flatiron School",
@"Location":@{@"StreetAddress":@"11 Broadway",@"City":@"New York",
@"State:@"NY",@"Zip":@"10004"}};

NSString *schoolZip = venue[@"Location"][@"Zip"];
```

Above we can see how to nest the creation of `NSDictionary` literals and then retrieve data within a nested `NSDictionary`.


##Nested `NSArray` objects

######Example
```
NSArray *courses = @[@[@"iOS Beginner",@"iOS Intermediate",@"iOS Advanced"], 
@[@"Android Full time",@"Android After school", @"Android Nights and weekends"]];

NSString *firstAndroidCourse = courses[2][1];

```
Above we can see an example of both nesting `NSArray` literals and then extract data from them.

##Nested combination of `NSDictionary` and `NSArray` objects


######Example
```objc
NSDictionary *mary = @{@"name":@"Mary",@"age":@16};
NSDictionary *zach = @{@"name":@"Zach",@"age":@17};
NSArray *students = @[mary,zach];

NSNumber *maryAge = students[1][@"age"];

```
And above is an example of the combination of the two nested collections.

##Takeaways

The fact is, code is for humans. So don't get too fancy with nesting as you will quickly find yourself in "nesting hell", where it is difficult for another programmer to read (and likely for you to read when you come back to it down the line.)

There really is no performance gain made by nesting. It is purely an aesthetic. But you do want to understand it, even if you choose not to use it, as other programmers may use nesting.