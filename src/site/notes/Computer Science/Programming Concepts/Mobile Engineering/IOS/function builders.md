---
{"dateCreated":"2022-04-27 16:59","tags":["swift"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/programming-concepts/mobile-engineering/ios/function-builders/","dgPassFrontmatter":true}
---


# function builders
_Functions builders_ is the language feature first introduced in Swift 5.1. It powers SwiftUI declarative [DSL](https://www.swiftbysundell.com/articles/building-dsls-in-swift/), which allows us to construct heterogeneous user interface hierarchies in a readable and concise way. In this article, we will learn how to utilize them in our code, covering the following topics:

##  Understanding Swift Function Builders
A **function builder** is a type that implements an embedded [[DSL\|DSL]] for collecting partial results from the expression-statements of a function and combining them into a return value.

The minimal _function builder type_ implementation looks like:

``` swift 
@_functionBuilder struct Builder {
    static func buildBlock(_ partialResults: String...)->String   
    {
        partialResults.reduce("", +)
    }
}
```

A _function builder type_ must be annotated with the `@functionBuilder` attribute, which allows it to be used as a custom attribute on its own.

The method `buildBlock()` is mandatory. It must be static and must have precisely this name. Otherwise, you’ll see a compilation error at a point of use.

A custom _function builder attribute_ can be applied to:

1.  A `func`, `var`, or [[subscript\|subscript]] declaration which is not a part of a protocol requirement. It causes the _function builder transform_ to be applied to the body of the function.
2.  A closure parameter of a function, including protocol requirements. It causes the _function builder transform_ to be applied to the body of any explicit closures that are passed as the corresponding argument unless the closure contains a return statement.
3. 
Let’s continue with our `@Builder` example and review both usage scenarios.

We can use special syntax inside the declarations by marking them with the attribute `@Builder`:

```swift
@Builder func abc() -> String {
    "Method: "
    "ABC"
}

struct Foo {
    @Builder var abc: String {
        "Getter: "
        "ABC"
    }

    subscript(_ anything: String) -> String {
        @Builder get {
            "Subscript: "
            "ABC"
        }
        set { /* nothing */ }
    }
}
```

If we invoke the declarations:

```
print(abc())

print(Foo().abc)
print(Foo()[""])
```

It will print:

```
Method: ABC
Getter: ABC
Subscript: ABC
```

Moving to the second scenario, here is how we can pass a function-builder-annotated closure as an argument:

```
func acceptBuilder(@Builder build: () -> String) {
    print(build())
}
```

Then call the `acceptBuilder()` function with the DSL syntax enabled:

```
acceptBuilder {
    "Closure argument: "
    "ABC"
}
```

The above code will print:

```
Closure argument: ABC
```


## The Purpose of Swift Function Builders
The class of problems that Swift function builders solve is the construction of hierarchal heterogeneous data structures. Some examples are:

-   Generating structured data, e.g., XML, JSON.
-   Generating GUI hierarchies, e.g., SwiftUI, HTML.

## Anatomy of Swift Function Builders
If we dump the generated AST([Abstract syntax tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree)) from the `abc()` method: 

```
(func_decl range=[builder.swift:10:10 - line:13:1] "abc()" interface type='() -> String' access=internal
...
  (declref_expr implicit type='(Builder.Type) -> (String...) -> String' location=builder.swift:10:31 range=[builder.swift:10:31 - line:10:31] decl=builder.(file).Builder.buildBlock@builder.swift:5:17 function_ref=single)
  ...
    (string_literal_expr type='String' location=builder.swift:11:5 range=[builder.swift:11:5 - line:11:5] encoding=utf8 value="Method: " builtin_initializer=Swift.(file).String extension.init(_builtinStringLiteral:utf8CodeUnitCount:isASCII:) initializer=**NULL**)
    (string_literal_expr type='String' location=builder.swift:12:5 range=[builder.swift:12:5 - line:12:5] encoding=utf8 value="ABC" builtin_initializer=Swift.(file).String extension.init(_builtinStringLiteral:utf8CodeUnitCount:isASCII:) initializer=**NULL**)
...
```

We’ll discover that it translates into the call to `Builder.buildBlock("Method: ", "ABC")`.

During the [semantic analysis](https://swift.org/swift-compiler/#compiler-architecture) phase, the Swift compiler [applies](https://github.com/apple/swift/blob/51267754fc35ca17d6bbb8f9066390925cbb5c02/lib/Sema/CSApply.cpp#L5059) the [function builder transforms](https://github.com/apple/swift/blob/78880ffc1a6bdbb01610990ba395e2cbef5d14a3/lib/Sema/BuilderTransform.cpp) to the parsed AST as if we had written `Builder.buildBlock(<arguments>)`

Another usage case is when a function builder is applied to a closure parameter. In this case, the Swift compiler will [rewrite the closure](https://github.com/apple/swift/blob/51267754fc35ca17d6bbb8f9066390925cbb5c02/lib/Sema/CSApply.cpp#L7713) to a closure with a [single expression body](https://github.com/apple/swift/blob/78880ffc1a6bdbb01610990ba395e2cbef5d14a3/lib/Sema/BuilderTransform.cpp#L784) containing the builder invocations.

To be of some use, a function builder must provide a subset of seven building methods that implement different kinds of transformations.

-   `buildBlock(_ parts: PartialResult...) -> PartialResult` combines multiple partial results into one.
-   `buildDo(_ parts: PartialResult...) -> PartialResult` same as `buildBlock()`, but for the `do` clause.
-   `buildIf(_ parts: PartialResult...) -> PartialResult` same as `buildBlock()`, but for the `if` statement.
-   `buildEither(first: PartialResult) -> PartialResult` and `buildEither(second: PartialResult) -> PartialResult` create partial results from the result of either of two optionally-executed sub-blocks. You must implement both of the methods and they must be the inverse of each other.
-   `buildExpression(_ expression: Expression) -> PartialResult` creates a partial result from a single expression.
-   `buildOptional(_ part: PartialResult?) -> PartialResult` creates a partial result from the result of an optionally-executed sub-block.
-   `buildFinalResult(_ parts: PartialResult...) -> Result` produces a final result out of multiple partial results.

> All of the methods support overloads based on their parameter types.

At a point of use, the Swift compiler will attempt to rewrite the DSL syntax using the provided subset of methods. In case that the compiler cannot not find a match, it will emit a compilation error.

## Implementing Custom Function Builder
Let’s sharpen our knowledge by implementing an `NSAttributedString` function builder.

The builder creates a final `NSAttributedString` out of substrings:

``` swift
@_functionBuilder
struct AttributedStringBuilder {
    static func buildBlock(_ components: NSAttributedString...) -> NSAttributedString {
        let result = NSMutableAttributedString(string: "")
        components.forEach(result.append)
        return result
    }
}
```

Next, we need to consider what types of expressions the builder supports. In our example, we will accept strings and images, and lift them to attributed substrings:

``` swift
@_functionBuilder
struct AttributedStringBuilder {
    ...
    
    static func buildExpression(_ text: String) -> NSAttributedString {
        NSAttributedString(string: text, attributes: [:])
    }
    
    static func buildExpression(_ image: UIImage) -> NSAttributedString {
        let attachment = NSTextAttachment()
        attachment.image = image
        return NSAttributedString(attachment: attachment)
    }
    
    static func buildExpression(_ attr: NSAttributedString) -> NSAttributedString {
        attr
    }
}
```

Note that we also accept expressions of type `NSAttributedString` and return them unmodified.

We’ll also need a helper method that allows us to add extra attributes:

``` swift 
extension NSAttributedString {
    func withAttributes(_ attrs: [NSAttributedString.Key: Any]) -> NSAttributedString {
        let copy = NSMutableAttributedString(attributedString: self)
        copy.addAttributes(attrs, range: NSRange(location: 0, length: string.count))
        return copy
    }
}
```

Then add a builder-based convenience initializer:

``` swift 
extension NSAttributedString {
    convenience init(@AttributedStringBuilder builder: () -> NSAttributedString) {
        self.init(attributedString: builder())
    }
}
```

Lastly, let’s test the builder. Note that SwiftUI does not directly support `NSAttributedString`s, hence we will resort to the good old UIKit:

``` swift
class ViewController: UIViewController {
    @IBOutlet weak var label: UILabel!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        label.attributedText = NSAttributedString {
            // 1.
            NSAttributedString {
                // 2.
                "Folder "
                // 3.
                UIImage(systemName: "folder")!
                // 4.
            }
            "\n"
            NSAttributedString {
                "Document "
                UIImage(systemName: "doc")!
                    .withRenderingMode(.alwaysTemplate)
            }
            .withAttributes([
                .font: UIFont.systemFont(ofSize: 32),
                .foregroundColor: UIColor.red
            ])
        }
    }
}
```

The Swift compiler translates the above code into the following `AttributedStringBuilder` method calls:

1.  `buildExpression()` with the `NSAttributedString` argument.
2.  `buildExpression()` with the `String` argument.
3.  `buildExpression()` with the `Image` argument.
4.  After all partial results have been built, the method `buildBlock()` is invoked with all intermediate substrings as arguments.

The result is next:
![Pasted image 20220428102925.png](/img/user/Assets/Pasted%20image%2020220428102925.png)

