# Best practices

## Xcode

### Version

The recommended version of Xcode (and Swift) for all purposes is the current available [in the App Store](https://itunes.apple.com/no/app/xcode/id497799835?mt=12). Please use the latest version unless otherwise stated.

### Project structure

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. The code should be grouped by type and feature for greater clarity.

To sync physical files with Xcode project files, you can use this [Synx](https://github.com/venmo/synx).

### Warnings

Enable warnings by adding `-Weverything` to your Build Settings under "Other Compiler Flags". If you need to ignore a specific warning you can use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) or add `-Wno-warning-to-be-disabled` (for example `-Wno-gnu-conditional-omitted-operand`).

`Please treat warnings as errors and try to minimize it.`

## Deployment

### Semantic Versioning

We support [semantic versioning](http://semver.org/), and it's important that minor releases are backwards compatible otherwise don't feel shy to make it a major release.

When making backwards compatible changes, flag your old APIs as deprecated like this:

```objc
- (NSInteger)foo:(NSInteger)bar __attribute__((deprecated("Use fooWithBar: instead")));
```


## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

`Treat comments as your inability to write clear code and minimise its use.`


## Blocks, delegates or data source

### Block/Closure
- Asynchronous (For example: networking operations)
- User inputs with multiple options (For example: UIAlertView's YES and NO)
- Data source driven inputs (For example: A table items with action blocks that were defined in the data source)
- Returns many values (For example looking for a field in a collection and returning the field and the indexPath)
- If there’s no tracked state or if state it’s defined in the same method

### Delegate
- Synchronous (For example: buttons actions in views that should perform on their parents)
- Shouldn't return values
- Provides control over performing an action (For example: UITextField's shouldEndEditing)
- User input with one action (For example: buttons actions in views that should perform on their parents)
- If tracked state is shared (if state is stored in a property or a constant)

### Data source
- Returns ONE value

# Swiftlint 

Swiftlint is a linter. its a tool that will help us to identify parts of code that is not following community standards or our custom styling guide.

#### Why we use swiftlint

* Consistency within a single project

* Consistency between projects

* Consistency across teams

To ensure all these consistency. We have a small [swift style guide](swift-style-guide.md) for our team but still we forgot to follow these rules. this linter will enforce you to follow style guide.
	
	
#### Installation
* Using [Homebrew](https://brew.sh/)
```shell
$ brew install swiftlint
```	
* Using CococaPods
Simply add the following line to your Podfile:
```shell
pod 'SwiftLint'
```
but we will use Homebrew method 


#### Running 
	We have two options to run swiftlint
* Terminal 
	go to project directory and type following command 
```shell
$ swiftlint
```	
* Integrate SwiftLint with Xcode
Add a new "Run Script Phase" to your project's default target. 
```shell
if which swiftlint >/dev/null; then
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```
To add a new "Run Script Phase" select Target, Build Phases, click on + icon on top side and select New Run Script Phase  

![Runscript](img/RunScript.jpg)

Now build your project. 


Swiftlint will lint using default rules, if you want to customize the rule place a .swiftlint.yml file in project directory

we will use our [Default swiftlint](swiftlint.yml) file. which is written on our [swift style guide](swift-style-guide.md)   

#### Autocorrect 

Swiftlint has a feature of autocorrect, which comes handy to correct some silly mistakes in our code 

to use this feature type following in terminal 
```shell
$ swiftlint autocorrect
```

Some types of violation that can be fixes with autocorrect 

* Closing brace.
* Colon.
* Comma Spacing.
* Opening Brace Spacing. 
* Trailing Newline.
* Trailing Semicolon.

for more type following in terminal 

```shell
$ swiftlint rules

```
Reference:
[BakkenBaeck](https://github.com/bakkenbaeck/iOS-playbook/blob/master/BEST_PRACTICES.md)