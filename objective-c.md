---
layout: page
title: Objective-C
header: Objective-C Style Guide
group: navigation
---
{% include JB/setup %}

Inspired by (and stolen shamelessly from) [NYTimes Objective-C Style
Guide](https://github.com/NYTimes/objective-c-style-guide) and [GitHub's
Coding conventions for Objective-C
projects](https://github.com/github/objective-c-conventions) (with some
personal bias thrown in for good measure).
Where the guide is silent, default to [Apple's Coding Guidelines for
Cocoa](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html).
Failing that, follow [Kernighan & Ritchie C style](http://en.wikipedia.org/wiki/The_C_Programming_Language).

## Table of Contents

* [Organization](#organization)
* [Spacing](#spacing)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Conditionals](#conditionals)
* [Variables](#variables)
* [Naming](#naming)
* [Comments](#comments)
* [Literals](#literals)
* [Constants](#constants)
* [Booleans](#booleans)
* [Enumerated Types](#enumerated-types)
* [Common Method Structures](#common-method-structures)
* [Xcode Project](#xcode-project)

## Organization
* Use `#pragma mark`s to categorize methods into functional groupings and protocol implementations, following this general structure:

    ```objc
    #pragma mark - lifecycle

    + (id)objectWithThing:(id)thing
    ...
    - (id)init
    ...

    #pragma mark - drawing

    - (void)drawRect:(CGRect)
    ...

    #pragma mark - SuperclassName

    - (void)someOverriddenMethod
    ...

    #pragma mark - NSCopying

    - (id)copyWithZone:(NSZone *)zone
    ...

    #pragma mark - NSObject

    - (NSString *)description {}
    ...
    ```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Never use spaces between parentheses and their contents.
* Asterisks indicating pointers belong with the variable. They should have one space to their left and none to their right.

    ```objc
    // bad
    + (NSString*)aStringMethod:(NSString * )inputString
    {
        NSString*temporaryStringOne;
        NSString* temporaryStringTwo;
        NSString * temporaryStringThree;
        ...

    // good
    + (NSString *)aStringMethod:(NSString *)inputString
    {
        NSString *temporaryStringOne;
        NSString *temporaryStringTwo;
        NSString *temporaryStringThree;
        ...
    ```

* Separate binary operands with a single space, but unary operands and casts with none.

    ```objc
    // bad
    NewType a = (NewType) b;

    for (int i=0; i<10; i ++) {
        doCoolThings();
    }

    // good
    NewType a = (NewType)b;

    for (int i = 0; i < 10; i++) {
        doCoolThings();
    }
    ```

* Method signatures require a space between the scope (`+`/`-`) indicator and the return type. Each method segment should be on its own line, with the colons aligned vertically. Both braces surrounding the method's implementation occupy their own lines.

    ```objc
    // bad
    -(BOOL)methodThatTakesAString:(NSString *)string andAnInteger:(NSInteger)integer{
        // implementation
        return answer;}

    // good
    - (BOOL)methodThatTakesAString:(NSString *)string
                      andAnInteger:(NSInteger)integer
    {
        // implementation
        return answer;
    }
    ```

* Other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement (preceded by a space) but close on a new line. `else` occupies the same line as the `if`'s closing brace.

    ```objc
    // bad
    if (user.isHappy){
        ...
    }
    else
    {
        ...
    }

    // also bad
    if (user.isHappy)
    {
        ...
    }
    else { ... }

    // good
    if (user.isHappy) {
        ...
    } else {
        ...
    }
    ```

## Dot-Notation Syntax

* Dot-notation should always be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

    ```objc
    // bad
    [view setBackgroundColor:[UIColor orangeColor]];
    UIApplication.sharedApplication.delegate;

    // good
    view.backgroundColor = [UIColor orangeColor];
    [UIApplication sharedApplication].delegate;
    ```

## Conditionals

* Conditional bodies should always use braces even when a conditional body could be written (on a single line) without braces to prevent errors. (See [the NYT guide](https://github.com/NYTimes/objective-c-style-guide/blob/master/README.md#conditionals) for examples of errors.)

    ```objc
    // bad
    if (!error)
        return success;

    // also bad
    if (!error) return success;

    // good
    if (!error) {
        return success;
    }
    ```

* The ternary operator `?` should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an if statement, or refactored into instance variables. The condition of a ternary expression should be enclosed in parentheses for clarity.

    ```objc
    // bad
    result = a > b ? x = c > d ? c : d : y;

    // good
    result = (a > b) ? x : y;
    ```

## Variables

* Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc.), `dealloc` methods and within custom setters and getters.

    ```objc
    // bad
    @interface someClass : NSObject
    {
        NSString *_label;
    }

    // good
    @interface someClass : NSObject

    @property (nonatomic) NSString *label;

    @end
    ```

## Naming

* Long, descriptive method names are good.
* Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for` loops.
* Apple naming conventions should be adhered to wherever possible (camelCase for variables, method names, and method name segments, CamelCase with an initial capital for class names, constant variables, and enumerated types).
* Property names should be camelCase. **If Xcode can automatically synthesize the variable, then let it.** Otherwise, in order to be consistent, the backing instance variables for these properties should have _camelCase name with a leading underscore and lowercase letter. This is the same format as Xcode's default synthesis.
* When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`. Local variables should not begin with underscores.

## Comments

* Comments should be used to explain **why** a particular piece of code does something, not **what** it does. Comments must be kept up-to-date or deleted.
* Block comments should generally be avoided. If you find yourself writing block comments, consider improving the clarity of the code itself instead.

## Literals

* `NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special attention to prevent `nil` values from being passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.
* Spaces belong between the braces of array and dictionary literals and their contents; between each member of the collection; and between keys, colons, and values of a dictionary literal.
* Long or complex array and dictionary literals may be broken into several lines.

    ```objc
    // bad
    NSString *greeting = [NSString stringWithUTF8String:"hello"];
    NSArray *names = [NSArray arrayWithObjects:@"Joe", @"Mark", @"Michael", @"Mike", @"Shivani", nil];
    NSDictionary *appOwners = [NSDictionary dictionaryWithObjectsAndKeys: @"Mike", @"iScout", @"Michael", @"Scorekeeper", @"Mark", @"Tournament", nil];
    NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
    NSNumber *buildingZIPCode = [NSNumber numberWithInteger:55413];

    // good
    NSString *greeting = @"Hello";
    NSArray *names = @[ @"Joe", @"Mark", @"Michael", @"Mike", @"Shivani" ];
    NSDictionary *productManagers = @{ @"iScout" : @"Mike",
                                       @"Scorekeeper" : @"Michael",
                                       @"Tournament" : @"Mark" };
    NSNumber *shouldUseLiterals = @YES;
    NSNumber *buildingZIPCode = @10018;
    ```

## Constants

* Constants are preferred over in-line string literals or numbers. Constants should be declared as `static const` variables and not `#define`s unless explicitly being used as a macro.

    ```objc
    // bad
    #define COMPANY_NAME @"Sport Ngin"
    ...
    [SNLSyncTimer timerWithInterval:10
                        maxInterval:900
                           hostname:@"api.stage.ngin-staging.com"
                            handler:^{ [self pull]; }];

    // good
    static const NSString * CompanyName = @"SportNgin";
    ...
    static const NSInteger TimerInterval = 10;
    static const NSInteger MaxInterval = 900;
    static const NSString *APIHostName = 10;
    ...
    [SNLSyncTimer timerWithInterval:TimerInterval
                        maxInterval:MaxInterval
                           hostname:@"api.stage.ngin-staging.com"
                            handler:^{ [self pull]; }];
    ```

## Booleans

* Use the full-caps `BOOL` to indicate boolean types.
* Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

    ```objc
    // bad
    if (someObject == nil) ...
    if ([someObject boolValue] == NO) ...
    if (isAwesome == YES) ...

    // good
    if (!someObject) ...
    if (![someObject boolValue]) ...
    if (isAwesome) ...
    ```

* If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor.

    ```objc
    @property (assign, getter=isEditable) BOOL editable;
    ```

## Enumerated Types

* Use the `NS_ENUM()` macro to define `enum`s.
* Each enumerated value should begin with the type name.
* The first value should be 1 unless it represents some sort of "false" state, in which case 0 is appropriate.

    ```objc
    typedef NS_ENUM(NSInteger, PeriodType) {
        PeriodTypeWarmup = 1,
        PeriodTypeNormal,
        PeriodTypeIntermission,
        PeriodTypeOvertime,
        PeriodTypeShootout,
        PeriodTypeEnd
    };
    ```

## Common Method Structures

* `init` methods should be structured like this:

    ```objc
    - (instancetype)init
    {
        self = [super init]; // or call the designated initalizer
        if (self) {
            // custom initialization
        }

        return self;
    }
    ```

* Singleton objects should use a thread-safe pattern for creating their shared instance.

    ```objc
    + (instancetype)sharedInstance
    {
      static id __sharedInstance = nil;
      static dispatch_once_t onceToken;
      dispatch_once(&onceToken, ^{
          __sharedInstance = [[self alloc] init];
      });

      return __sharedInstance;
    }
    ```

## Xcode Project

* The filesystem directories should be kept in sync with the Xcode file groups.
* When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).
