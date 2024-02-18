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
![Pasted image 20220428102925.png](data:image/png;base64,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)

