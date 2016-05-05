# Simple Code Style

>简明代码命名规范
 main reference Google Code Style

|  item |项目|C++|Java|Perl|
|-------|-----|-----|-----|-----|
|Package|包路径|-|all lowercase|-|
|file|文件|-|UpperCamelCase|-|
|var|变量|-|lowerCamelCase|-|
|method|函数|-|lowerCamelCase|-|
| Column limit|行长度|-| 80 or 100 characters|-|


## C/C++
[cpp guide](https://google.github.io/styleguide/cppguide.html)

**Naming**

The most important consistency rules are those that govern naming. The style of a name immediately informs us what sort of thing the named entity is: a type, a variable, a function, a constant, a macro, etc., without requiring us to search for the declaration of that entity. The pattern-matching engine in our brains relies a great deal on these naming rules.

Naming rules are pretty arbitrary, but we feel that consistency is more important than individual preferences in this area, so regardless of whether you find them sensible or not, the rules are the rules.

#### General Naming Rules

Names should be descriptive; eschew abbreviation.

Give as descriptive a name as possible, within reason. Do not worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. Do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word.

```
int price_count_reader;    // No abbreviation.
int num_errors;            // "num" is a widespread convention.
int num_dns_connections;   // Most people know what "DNS" stands for.
int n;                     // Meaningless.
int nerr;                  // Ambiguous abbreviation.
int n_comp_conns;          // Ambiguous abbreviation.
int wgc_connections;       // Only your group knows what this stands for.
int pc_reader;             // Lots of things can be abbreviated "pc".
int cstmr_id;              // Deletes internal letters.
```

## Java

[java guide](https://google.github.io/styleguide/javaguide.html)

##### 5.2.1 Package names

Package names are **all lowercase**, with consecutive words simply concatenated together (no underscores).
For example,
```
com.example.deepspace
```
 not com.example.deepSpace or com.example.deep_space.

##### 5.2.2 Class names

Class names are written in UpperCamelCase.

Class names are typically nouns or noun phrases.
For example, Character or ImmutableList.
Interface names may also be nouns or noun phrases (for example, List), but may sometimes be adjectives or adjective phrases instead (for example, Readable).

There are no specific rules or even well-established conventions for naming annotation types.

Test classes are named starting with the name of the class they are testing,
and ending with Test. For example, HashTest or HashIntegrationTest.

##### 5.2.3 Method names

Method names are written in **lowerCamelCase**.

Method names are typically verbs or verb phrases.
For example, sendMessage or stop.

Underscores may appear in JUnit test method names to separate logical components of the name.

One typical pattern is test<MethodUnderTest>\_<state>,for example testPop_emptyStack.
There is no One Correct Way to name test methods.

## Shell

**Naming Conventions**

*Function Names*

link ▽ Lower-case, with underscores to separate words. Separate libraries with ::. Parentheses are required after the function name. The keyword function is optional, but must be used consistently throughout a project.
If you're writing single functions, use lowercase and separate words with underscore. If you're writing a package, separate package names with ::. Braces must be on the same line as the function name (as with other languages at Google) and no space between the function name and the parenthesis.
```
# Single function
my_func() {
  ...
}

# Part of a package
mypackage::my_func() {
  ...
}
```
The function keyword is extraneous when "()" is present after the function name, but enhances quick identification of functions.

*Variable Names*

link ▽ As for function names.
Variables names for loops should be similarly named for any variable you're looping through.
```
for zone in ${zones}; do
  something_with "${zone}"
done
```
*Constants and Environment Variable Names*

link ▽ All caps, separated with underscores, declared at the top of the file.
Constants and anything exported to the environment should be capitalized.

```
# Constant
readonly PATH_TO_FILES='/some/path'
```

**Both constant and environment**
```
declare -xr ORACLE_SID='PROD'
```
Some things become constant at their first setting (for example, via getopts). Thus, it's OK to set a constant in getopts or based on a condition, but it should be made readonly immediately afterwards. Note that declare doesn't operate on global variables within functions, so readonly or export is recommended instead.

```
VERBOSE='false'
while getopts 'v' flag; do
  case "${flag}" in
    v) VERBOSE='true' ;;
  esac
done
```

readonly VERBOSE
*Source Filenames*

link ▽ Lowercase, with underscores to separate words if desired.
This is for consistency with other code styles in Google:
```
maketemplate or make_template but not make-template.
```

*Read-only Variables*

link ▽ Use readonly or declare -r to ensure they're read only.
As globals are widely used in shell, it's important to catch errors when working with them. When you declare a variable that is meant to be read-only, make this explicit.
```
zip_version="$(dpkg --status zip | grep Version: | cut -d ' ' -f 2)"
if [[ -z "${zip_version}" ]]; then
  error_message
else
  readonly zip_version
fi
Use Local Variables
```
link ▽ Declare function-specific variables with local. Declaration and assignment should be on different lines.
Ensure that local variables are only seen inside a function and its children by using local when declaring them. This avoids polluting the global name space and inadvertently setting variables that may have significance outside the function.

Declaration and assignment must be separate statements when the assignment value is provided by a command substitution; as the 'local' builtin does not propagate the exit code from the command substitution.

```
my_func2() {
  local name="$1"

  # Separate lines for declaration and assignment:
  local my_var
  my_var="$(my_func)" || return

  # DO NOT do this: $? contains the exit code of 'local', not my_func
  local my_var="$(my_func)"
  [[ $? -eq 0 ]] || return

  ...
}
```

*Function Location*

link ▽ Put all functions together in the file just below constants. Don't hide executable code between functions.
If you've got functions, put them all together near the top of the file. Only includes, set statements and setting constants may be done before declaring functions.

Don't hide executable code between functions. Doing so makes the code difficult to follow and results in nasty surprises when debugging.

*main*

link ▽ A function called main is required for scripts long enough to contain at least one other function.
In order to easily find the start of the program, put the main program in a function called main as the bottom most function. This provides consistency with the rest of the code base as well as allowing you to define more variables as local (which can't be done if the main code is not a function). The last non-comment line in the file should be a call to main:
```
main "$@"
```
Obviously, for short scripts where it's just a linear flow, main is overkill and so is not required.

## Python

**Names to Avoid**

single character names except for counters or iterators
dashes (-) in any package/module name
__double_leading_and_trailing_underscore__ names (reserved by Python)

**Naming Convention**

"Internal" means internal to a module or protected or private within a class.
Prepending a single underscore (_) has some support for protecting module variables
and functions (not included with import * from).
Prepending a double underscore (__) to an instance variable or method effectively serves
to make the variable or method private to its class (using name mangling).
Place related classes and top-level functions together in a module.
Unlike Java, there is no need to limit yourself to one class per module.
Use CapWords for class names, but lower_with_under.py for module names.
Although there are many existing modules named CapWords.py, this is now discouraged because it's confusing when the module happens to be named after a class.
 ("wait -- did I write import StringIO or from StringIO import StringIO?")
Guidelines derived from Guido's Recommendations


|Type	|Public|	Internal|
|----|----|----|----|
|Packages|	lower_with_under||
|Modules |	lower_with_under|	_lower_with_under|
|Classes |	CapWords|	_CapWords|
|Exceptions |	CapWords||
|Functions |	lower_with_under()	|_lower_with_under()|
|Global/Class Constants	| CAPS_WITH_UNDER	|_CAPS_WITH_UNDER|
|Global/Class  Variables |	lower_with_under|	_lower_with_under
|Instance Variables|	lower_with_under|	_lower_with_under (protected) or __lower_with_under (private)
|Method Names	|lower_with_under()	|_lower_with_under() (protected) or __lower_with_under() (private)
|Function/Method| Parameters|	lower_with_under|
|Local | Variables |	lower_with_under |
