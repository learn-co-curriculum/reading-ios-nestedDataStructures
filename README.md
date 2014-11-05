#Nested Data Structures

As you start to look at other people's code, as well as get more comfortable with writing your own code, you will begin to encounter and write nested data structures. Take a few minutes to review examples of the following common nested method calls, dictionaries, and arrays.

##Methods

######Example
```objc
[[NSNumber numberWithFloat:23.0] stringValue];
```

These nested method calls first convert a `CGFloat` to an `NSNumber` and then convert the `NSNumber` in the outer method call to an `NSString`.

Method calls can be nested infinitely, but it is usually smart not to nest more than a couple as it will become hard to read quickly, even for the person who wrote the code if they have to come back to it in a week and modify it!

##Dictionaries
```objc
NSDictionary *venue = {@"name":@"Flatiron School",@"Location":@{@"StreetAddress":@"11 Broadway",@"City":@"New York",@"State:@"NY",@"Zip":@"10004"}};

NSString *schoolZip = venue[@"Location"][@"Zip"];
```

Above we can see how to nest the creation of NSDictionary literals and then retrieve data within a nested dictionary.


##Arrays
```
NSArray *courses = @[@[@"iOS Beginner",@"iOS Intermediate",@"iOS Advanced"], @[@"Android Full time",@"Android After school", @"Android Nights and weekends"]];

NSString *firstAndroidCourse = courses[2][1];

```
Above we can see an example of both nesting `NSArray` literals and then extract data from them.

##Dictionaries mixed with arrays

```objc
NSDictionary *mary = @{@"name":@"Mary",@"age":@16};
NSDictionary *zach = @{@"name":@"Zach",@"age":@17};
NSArray *students = @[mary,zach];

NSNumber *maryAge = students[1][@"age"];

```
And above is an example of the combination of the two nested collections.