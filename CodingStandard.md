**TrickFire Robotics Design and Coding Standard**

Purpose
=======================================================================================================================

This document is intended to create a standard guideline for promoting code consistency and quality.
Developed for use with the **University of Washington Bothell Trickfire Robotics Team**

NASA Robotic Mining Competition, 2018


Using This Document
=======================================================================================================================

During a code review, reviewers may wish to consult this page to help determine if the source code aligns with the
project coding standards, guidelines, and conventions.

If a scenario is not covered on this page, use good judgment and remember *consistency is king*.  If there is an
established way of doing something in other parts of the code base, that's probably the way it should be done.

If the same question is likely to arise again later in the project, notify the Lead Software Engineer, so he or she can
update this page for everyone's benefit.

Note that the use of constructs or mechanisms for which justification is required is not discouraged. Requiring 
justification in these cases is simply intended to prevent misuse and to capture rationale for future reference.

Because the Trickfire Robotics NASA RMC 2018 codebase is built on top of the Robot Operating System framework, any
topic not covered in this document is deferred to the official ROS C++ style guide, found here:
<http://wiki.ros.org/CppStyleGuide>

Any topic not covered in either this document or the official ROS style guide is deferred to the official Google C++
Style Guide, found here: <https://google.github.io/styleguide/cppguide.html>

If a topic is not covered in any of these documents, but it seems that it needs to be addressed, this document may (and
should) be modified to reflect the most up-to-date standard employed by the Trickfire Robotics Software Team.

General Standards
=======================================================================================================================

##Naming
All names should be descriptive of their purpose. Prefer long, descriptive names to short, confusing names.

**Files**

- Source files should be `lower_case`. C++ files should use either the `.h` or `.cpp` file extensions.

- In general, the use of extra prefixes for file names is discouraged:
  - Source files representing ROS Nodes should not contain a `node_` prefix or `node_` suffix.
  - Message (`.msg`), Service (`.srv`), and Action (`.action`) files should be `CamelCase`, not contain the respective
  "Message", "Service", or "Action" identifiers in the name.
  - Configuration (`.yaml`) files should not contain `_config` or `_param` suffixes.
  - Launch files (`.launch`) should not contain the `_launch` suffix.
- Exeptions:
  - Packages should be `lower_case` and be contain the `tfr_` prefix (as package names are global to the entire ROS
    ecosystem).
  - Files defining an abstract base class should contain a `_interface` suffix.

**Identifiers**

C++ code should adhere to the naming conventions below to promote consistency
throughout the project.
```
  - Pre-Processor Macros: `UPPER_CASE`
  - Class, Struct, Union, and Enum Types: `CamelCase`
  - Functions and Class Methods: `lower_case`
  - Compile-time constants (const, constexpr, enumerators, etc.): `kCamelCase`
  - Variables and Function Parameters: `lower_case`
  - constexpr Methods and Functions: `lower_case`
  - Non-const struct member variables: `lower_case`
  - Non-const class member variables: `m_lower_case`
  - Const member variables: `kCamelCase`
  - Template type parameters: `CamelCase`
  - Template value parameters: `kCamelCase`
  - Namespaces: `lower_case`

- Abstract base classes (interfaces) should be prefixed with I, as in
  `IContainer`.

- Acronyms and abbreviations should be avoided, unless they are well-known and
  commonly encountered throughout the project.
```
##Formatting

- All C++ and Python code should use 4 spaces instead of tabs.
- C++ braces should always be placed on their own line.
- Inline C++ functions inside of header definitions is discouraged.
- Ommitting braces in C++ clauses that include only a single statement is encouraged, unless that statement is 
  another C++ clause (such as a `for` loop or `if` statement).
- C++ and Python source code should be wrapped to 80 characters.
  - EXCEPTION: If this significantly hinders readability, it may be extended to 120 characters infrequently.
- XML, YAML, and Launch source files should be wrapped to 120 characters.
- When wrapping variable declarations, align input parameters if possible.

```
if (true)
    do_something();

if (true)
{
    do_something();
    do_something_else();
}

if (false)
{
    for (int i = 0; i < 10; i++)
        do_something();
    
    if (true)
    {
        ros::SomeLongNamespace::SomeType<AwkwardTemplate> var(param1, param2,
                                                              param3, param4);
    }
}
```

##Code

**Language Specification**

All production code should be written in the C++ programming language and should
be compliant with the C++14 (ISO/IEC 14882:2014) language standard, except where
such standards are not supported by the Robot Operating System (ROS) framework.
If such instances are found, notify the Software Director so that they may be
included in this document.

**Comments**

Comments must be used to clarify parts of code that merit explanation,
must be intelligible, and must be maintained.

*[Question: Should we add a standard here for including Doxygen as a supported 

tool?]*

**Compilation**

Code should compile and link with no errors or warnings at the `-Wall` `-Wextra`
warning level.  Unavoidable warnings may be disabled locally, in as small a
section of code as possible, with rationale stated in a comment wherever the
warning is disabled.


**#Includes**

System-wide include files (e.g. stdafx.h) must contain a minimal amount of
definitions.

#include directives in a file must only be preceded by other preprocessor
directives or comments. Exception: files that contain large data tables
(e.g. a CRC coefficient lookup table) may be included at any location to
promote readability.


**Header Files**

Every header file should be able to be compiled in isolation, meaning it
includes any headers that its contents depend upon.  Relying on transitive
#includes of other headers can make maintenance difficult and introduce
subtle issues.


**Memory**

A function must not return a pointer or reference to memory allocated
within that function with temporary or automatic storage duration.

Classes that allocate memory with dynamic storage duration, including
any memory allocated with the new operator, or allocate other shared resources
must wrap those members with smart pointers.  `std::shared_ptr` is preferred.


**Internal/External Linkage**

Variables with static storage duration that are not intended to be
referenced outside of their compilation unit must be declared with internal
linkage.

Variables (and functions and types) declared and used within a single
compilation unit should be declared inside an anonymous namespace, instead of
using the `static` keyword.

**Casting**

C++ style casts must be used. Other cast formats (such as C-style
casts) must not be used. Note that overloading typecast operators in classes is
allowed.

**Const**

Variables and class member functions must be qualified with the const
keyword where possible (with consideration to future changes). Exception:
Function parameter variables passed by value.

**Assignment Operator**

User defined copy assignment operators must always return a reference to
`*this`.

User defined copy assignment operators must be self-assignment safe.

User defined copy assignment operators in derived classes must explicitly
perform the assignment of its base class.

**Constructors**

Constructor initializer lists must be used instead of assignment
initialization in the constructor's body when possible.

**Virtual Destructors**

A class destructor must be virtual if and only if the class is
polymorphic (that is, it contains virtual methods).

**Operator Overloading**

Logical AND ("&&"), logical OR ("||") and the comma (",") operators
must never be overloaded.

**Derived Classes**

A derived class must never redefine a non-virtual function.

**Downcasting**

Downcasting must always be given justification.  Generally, a downcast represents 
a problematic design.

**Return Types**

Returning a non-const reference or pointer to non-public member data from a
public class member function requires justification.

**Function/Operator Overloading**

Function or operator overloading must only be used when performing the same
logical function.

**Inheritance**

Behavioral type consistency must be maintained between publicly derived
classes and base classes. That is, public inheritance must implement an
"is-a" relationship and subtypes (publicly derived classes) must be
substitutable.

Use of multiple inheritance or virtual inheritance (that is, using the virtual
keyword in the inheritance specification for a class to make one or more of its
parent classes be virtual base classes) requires justification.

**Complexity**

Code functions must not be overly complex. Readability is generally preferred 
when performance considerations are minor. If performance considerations are 
not minor, optimized code should still be written to be as readable as possible
and explained thorougly in code comments.

**Dead Code**

Production software must not include any dead code.

Dead code is: *Executable object code (or data) that is part of the production
software which, as a result of a design error cannot be executed or used in an
operational configuration of the target computer environment and is not
traceable to a system or software requirement.*

**Unused Code**

Unused source code must be prevented from inclusion in production software
by means of preprocessor directives, and clearly documented as unused source
code by means of code comments. Code statements must not be commented out in
production source code unless they are clearly part of the source code
documentation (e.g. examples).

Unused code is: *Compile time excluded non-executable source code. That is,
source code that is discarded by the compilation process and does not result in
production executable object code (or data).*

**Goto**

The `goto` keyword should not be used, except when absolutely required. Justification
must be provided when using the `goto` keyword.

**Variable Initialization**

Variables with automatic storage duration must be initialized before use.

**Global Variables**

Use of variables with external linkage ("global variables") should not be used.

**Recursion**

Code that implements recursive algorithms must be supported by a justification
showing that the execution time and memory requirements are bounded and
compatible with the target hardware and software architecture.

**Unused Return Values**

The return value of a non-void function must be explicitly cast to (void)
using a C-style cast if the value is not used by the calling function.

**Order of Operations**

Expressions using two or more operators of different precedence must use
parentheses to clarify the binding of operands to operators when the parsing
order is not intuitive.

**Types**

For integer types in C and C++ code, use int, unsigned int, plain char and bool
when possible, and don't use precise-width integer types in the absence of a
specific sizing requirement.

In other words, if a specific size is needed, a precise-width integer type
(e.g., uint16_t) from `<stdint.h>` or equivalent must be used, and the following
fundamental types must not be used: signed char, unsigned char, short, unsigned
short, long, unsigned long, long long, unsigned long long. Exception: These
types may be used to define precise-width types in `<stdint.h>` or equivalent.

Only these standard C++ fundamental numeric types should be used:

* `bool`
* `char` (7-bit ASCII character)
* `int` (32 bits, signed)
* `unsigned` (32 bits, unsigned)
* `size_t` (pointer size, 32 bits, unsigned)
* `ptrdiff_t` (pointer difference size, 32 bits, signed)
* `int8_t`
* `int16_t`
* `int32_t`
* `int64_t`
* `uint8_t`
* `uint16_t`
* `uint32_t`
* `uint64_t`
* `float`
* `double`

The C++ standard only guarantees the size of `int` and `unsigned` are at
least 16 bits, but we assume they are 32 bits, as is common for 32-bit
platforms.

Signed values (`int`) should be prefered for representing quantities.  Unsigned
values (`unsigned`) should not be used to "guarantee" that a function parameter
or other variable is non-negative.  Assertions should be used instead.

The C++ standard also does not specify the signedness of `char`, so is up to
the compiler implementer to choose or make it a configurable option.  Do not
rely on that choice.  Instead, use `int8_t`/`uint8_t` and use `char` only if
referring to a character in a plain C-style string.

Do not use `long double`.

**dynamic_cast**

Except for dynamic_cast, use of Run-Time Type Identification (RTTI) such as
the typeid operator requires justification.

**Friend**

Using the friend keyword requires justification.

**Initialization Lists**

Constructor initializer lists must have the same order as the member
data in the class definition.

**Construction**

Referencing objects within a constructor that are not member variables
or arguments to that constructor requires justification showing that these
objects have been constructed and initialized prior to the execution of the
constructor.

**Macros**

Use of C preprocessor function-macros requires justification. Use of
inline functions are preferred.

Software Design Standards
================================================================================

**Software Design Constraints**

- Global data must be documented in the design.
- Global data must be minimized.
- Bi-directional dependencies between software elements must be justified.
- Exceptions and assertions used for control flow are forbidden.
- The use of self-modifying software is prohibited.

**Software Design Guidelines**

- The architecture should support one or more levels of abstraction with clearly
  defined layers.
- The architecture and design detail should be modular.
- The design should reduce software complexity.
- The design should promote reuse.
- The design should facilitate extensibility.
- The design should enhance maintainability.
- The design should result in deterministic software with correct and
  predictable behavior.
- Justification will be required when considering software constructs or schemes
  such as scheduling, interrupt and event-driven architectures, dynamic tasking,
  re-entry, global data and exception handling.
- The design should have high cohesion within software elements.
- The design should have loose coupling between software elements.
- The design should include well-defined interface descriptions between software
  elements.
- The design should include a hardware abstraction layer (HAL) that provides a
  device-independent interface to hardware services.

**Object-Oriented Software Design Guidelines**

- Where appropriate, make use of proven design patterns. If a design pattern
  adds complexity, make sure the benefit outweighs the added complexity.
- Use of interfaces as a means to decrease coupling is recommended; allow
  clients to depend on interfaces to software elements, not on their
  implementations.
- Composition/aggregation is recommended instead of implementation inheritance.
- Use of inheritance solely to provision for unlikely future product
  improvements should be avoided. Abstracting an interface and separating it
  from an implementation as reasonable accommodation for change may be
  appropriate.
- Avoid allocating too many responsibilities to a single class.


Additional Guidelines and Conventions
================================================================================

This section contains recommendations, not project standards.

The guidelines should be considered during code review and deviations in the
code base should be justified or resolved as soon as possible.



**Header Guards**

All headers should have `#define` guards that match the file name.  The `#endif`
should be followed by a comment indicating the symbol.  For example,
some_file.h would be guarded as follows:

```c++
#ifndef SOME_FILE_H
#define SOME_FILE_H

   ...

#endif // SOME_FILE_H
```

[The #pragma once Macro accomplishes this same goal, but it is <a 

href="https://en.wikipedia.org/wiki/Pragma_once">none-standard</a>.]

**Header File Layout**

A typical .h file should look like this:

```c++
/*
 *  (Copyright and File Header)
 */
#ifndef FOOS_FOO_H
#define FOOS_FOO_H

namespace foos {

/*
 *  (Class Header)
 */
struct Widget
{
    int x{};
    int y{};
};

/*
 *  (Class Header)
 */
class Foo
{
public:
    Foo();
    explicit Foo(int bar);
    Foo(int bar, int id);
    Foo(Foo& other) = delete;
    Foo(Foo&& other) = delete;
    Foo& operator=(Foo& other) = delete;
    Foo& operator=(Foo other) = delete;
    ~Foo();
    int bar() const
    {
        return bar_;
    }
    void set_bar(int bar)
    {
        bar_ = bar;
    }
    int double_bar() const;

private:
    void compute_bar(int x);
    int bar_;
    int id_;
    Widget widget_;
}

/*
 *  (Routine Header)
 */
inline int Foo::double_bar() const
{
    return 2 * bar_;
}

} // namespace foos

#endif // FOOS_FOO_H
```

Sections should be ordered public, protected, then private.

Class members should be declared in this order:
 - Using-declarations, typedefs and enums
 - Constants (static const/constexpr data members)
 - Constructors and assignment operators
 - Destructor
 - Methods, including static methods
 - Member variables (except constant data members)

Function parameters should be declared in this order:
  - Input
  - Input/Output
  - Output


**Implementation File Layout**

A typical .cpp file should look like this:

```c++
/*
 *  (Copyright and File Header)
 */
#include "foo.h"
#include "library.h"
#include "other.h"
#include "core.h"

namespace foos {

/*
 *  (Routine Header)
 */
Foo::Foo() : bar_{}, foo_{}, widget_{}
{
}

/*
 *  (Routine Header)
 */
Foo::Foo(int bar): bar_{bar}, id_{}, widget_{}
{
}

/*
 *  (Routine Header)
 */
Foo::Foo(int bar, int id): bar_{bar}, id_{id}, widget_{}
{
}

/*
 *  (Routine Header)
 */
Foo::~Foo() = default;

/*
 *  (Routine Header)
 */
void Foo::compute_bar(int x)
{
    bar_ = 5 * x;
}

} // namespace foos

```

Member function definitions can appear in any order in the .cpp file, but
should be ordered logically to help the reader.

The #include for the class header should be the first substantive line in the
source file, following the file header and the copyright block.

Source code should be wrapped at 80 characters.


**Inlining**

Trivial member functions can be defined directly inside the class definition
and do not require header comments.  They should not be marked `inline` as they
are considered inline by default.

Other simple member functions can be defined in a header file, but they should
be defined outside the class definition, with header comments.  They should be
marked `inline`.  This does not guarantee that the code will be inlined by the
compiler, but rather it prevents violations of the "One Definition Rule" and
link-time errors.

Template functions defined in a header file should not be marked `inline`
as they are considered inline by default.

**Special Member Functions**

The special member functions of a class (constructors, copy/move constructors,
destructors, and copy/move assignment operators) should be declared in the
class definition and defined below the class definition in the header file if
they are inline, or in the source file otherwise.

Undesired special member functions should be marked `= delete;` inside the
public section of the class definition (rather than making them `private`).

Except for simple struct types, default versions of all special member functions
should be explicitly declared and defined.  Declare them in the class definition
and define them using `= default;` in the source file.

For example:

```c++
/// a simple struct type can implictly define special
/// member functions and use default member initialization

/*
 * (struct header)
 */
struct Point
{
    double x{1.0};
    double y{1.0};
}

/// class defined in .h file

/*
 * (class header)
 */
class Position
{
public:
    Position() = delete;
    Position(int lat_deg, int lat_min, int lat_sec,
             int lon_deg, int lon_min, int lon_sec);
    double distance_from(const Position& position);
    ~Position();

private:
    double lat_;
    double lon_;
}

/// methods are inline and defined in .h file
/// or non-inline and defined in .cpp file

/*
 * (routine header)
 */
inline Position::Position(int lat_deg, int lat_min, int lat_sec,
                          int lon_deg, int lon_min, int lon_sec)
{
    // ...
}
/*
 * (routine header)
 */
inline Position::~Position() = default;
/*
 * (routine header)
 */
inline double Position::distance_from()
{
    // ...
}
```

Single argument constructors and conversion functions should be marked
`explicit` unless they are intended to participate in implicit conversions.  In
that case, provide rationale in the conversion function routine header.

**Variable Initialization**

Prefer C++11 uniform initialization syntax (curly braces) for initializing 
variables.

```c++
Foo  foo{};       // OKAY
Foo  foo;         // BAD
Foo  foo();       // BAD
Foo  foo = {};    // BAD

Foo  foo{1, 2};   // OKAY
Foo  foo(1, 2);   // BAD

int  x{};         // OKAY, initializes to zero
int  x;           // BAD,  uninitialized variable

int* x_ptr{};     // OKAY, initializes to nullptr
int  x_ptr;       // BAD,  uninitialized pointer

int x   = initialize_x();    // OKAY
Foo foo = initialize_foo();  // OKAY

int x;
Foo foo;
util::tie(x, foo) = get_tuple_of_x_and_foo();    // OKAY, if Foo isn't too large

int x;
Foo foo;
initialize_x_and_foo(x, foo);    // OKAY, but prefer return values
```

There may be times where uniform initialization syntax conflicts with
constructors taking an initializer_list, in which case, use the syntax
that provides the expected behavior.

**Member Initialization**

All member variables should be initialized in the initializer list of the
constructor(s) defined in the source file.

Default in-class member initializers can be used for simple types which have
only the default constructor.  In all other cases,
omit the in-class member initializers and use an initializer list.

OKAY - Class member initialization is specified in one place:
```c++
// .h file
struct Position
{
    double lat{90.0};
    double lon{180.0};
}
```

BAD - Class member initialization is specified in two places:
```c++
// .h file
class Position
{
public:
    Position();
    Position(int lat_deg, int lat_min, int lat_sec,
             int lon_deg, int lon_min, int lon_sec)
private:
    double lat_{90.0};
    double lon_{180.0};
}

// .cpp file
Position::Position = default();
Position::Position(int lat_deg, int lat_min, int lat_sec,
                   int lon_deg, int lon_min, int lon_sec)
: lat_{lms_to_deg(lat_deg, lat_min, lat_sec)}
, lon_{lms_to_deg(lon_deg, lon_min, lon_sec)}
{
}
```

OKAY - Class member initialization is specified in one place:
```c++
// .h file
class Position
{
public:
    Position();
    Position(int lat_deg, int lat_min, int lat_sec,
             int lon_deg, int lon_min, int lon_sec);
private:
    double lat_;
    double lon_;
}

// .cpp file
Position::Position()
: lat_{90.0}
, lon_{180.0}
{
}
Position::Position(int lat_deg, int lat_min, int lat_sec,
                   int lon_deg, int lon_min, int lon_sec)
: lat_{lms_to_deg(lat_deg, lat_min, lat_sec)}
, lon_{lms_to_deg(lon_deg, lon_min, lon_sec)}
{
}

```

Zero-initialize fundamental member types with empty brackets, for
consistency with default-initialized user-defined types.

```c++
class Navaid
{
public:
    Navaid();
private:
    String<4> id_;
    double lat_, lon_;
    Frequency frequency_;
}
Navaid::Navaid()
: id_{}         // --> default initialize string ("")
, lat_{}        // --> 0.0
, lon_{}        // --> 0.0
, frequency_{}  // --> default initialize Frequency object
{
}
```


**Namespaces**

Namespaces should be closed with a comment as in `} // namespace foos` or
`} // namespace` for an unnamed (anonymous) namespace.

Namespace contents (named or unnamed) should not be indented.

**Namespace Using Directives**

Namespace using directives (eg, `using namespace fplm;`) should not be used.

**Enum Classes**

The C++11 scoped `enum class` should be preferred over the unscoped C-style
`enum`.

**Null Values**

The C++11 `nullptr` keyword should be used for the value of an uninitialized
pointer instead of `0` or `NULL` value.  `'\0'` should be used for a null
character.

**Auto Keyword**

Use of the `auto` keyword is encouraged for declaring local variables where
it helps readability, but otherwise does not change the behavior of the code.

Non-local variables should not be declared using `auto`, including function
parameters or return types, except for creating a generic template-like
function.

**Assertions**

Run-time conditional checks (assertions) should be used liberally to help
expose software bugs in development and production builds.

The standard `assert(cond)` macro from `<cassert>` prints to standard output and
aborts the system if `cond` evaluates false.  When running on target hardware,
standard output will not be available, so `assert(cond)` should not be used.

Instead, a utility `ASSERT` macro should be created that is compatible with the
target device, and it should be used in place of `assert(cond)`.

```c++
#ifndef NDEBUG
    int index = compute_index(data);
    ASSERT(index > previous_index, "Invalid index");
#endif // NDEBUG
```

`ASSERT()` must only be used for detecting software design errors.  Hardware 

errors,
for example bit flips or failed register reads, should be detected and handled
through other means.

The correctness of the software also should not depend on the execution of the
condition.  In other words, assertion condition expressions cannot have side
effects.  For example,

```c++
Data d = compute_data();
CURIOUS(!is_crc_valid(d), "Invalid CRC");  // logs but does not terminate
return false;  // the correctness of the software must not depend
               // on whether is_crc_valid(d) is executed
```

**Logging**

Logging macros should also be created and applied generously throughout the code
to capture system state and behavior information.

Guidelines for Object-Oriented and Related Technologies
================================================================================

**Implementation Inheritance**

<u>Summary</u>

- Prefer composition over implementation inheritance for code reuse
- Prefer private or protected inheritance for implementation inheritance
- Do not redefine non-virtual members of a base class

<u>Discussion</u>

Implementation inheritance occurs whenever a base class having state data (non-
static member variables) or implemented member functions (either virtual or
non-virtual) is inherited by a derived class.  In particular, it is not
considered implementation inheritance if the base class contains no state data
and only pure-virtual member function declarations (no implementations).

In most cases, implementation inheritance should be expressed with private or
protected inheritance.  Public implementation inheritance can be dangerous
because the requirements of the base class may be overlooked by users of the
derived class.  As a result, the base class members could be put into an
invalid state.

Keeping implementation inheritance protected or private constrains the use of
the base class, precluding substitution, so users of the derived class need
be concerned only with the requirements of its public interface.  It is then
the responsibility of the derived class implementation to ensure the base
class requirements are met.

Composition (putting common code into a class that is reused as a member of
other classes) is generally preferable to implementation inheritance for ease
of maintainability.  Before choosing implementation inheritance, decide if
composition is a suitable alternative.  [^Sutter34]

If implementation inheritance is used, ensure that none of the non-virtual
base class member functions are redefined by the derived class.  Also ensure
that the correct base class constructor is called whenever a derived class
constructor is invoked.

Implementation inheritance relationships should be explained in the in the
`@inherits` field of the derived class header.

**Interface Inheritance**

<u>Summary</u>

- Public inheritance implies interface inheritance and represents an "is-a"
  relationship
- All publicly derived classes must meet the requirements of the base class
- Interface inheritance must satisfy the Liskov Substitution Principle
- Avoid mixing interface inheritance and implementation inheritance if possible
- Consider using abstract base classes
- Do not redefine non-virtual member functions
- Mark function overrides with `override` or `final`, leaf classes `final`
- Do not call virtual functions in constructors or destructors
- Document inheritance rationale in the `@inherits` field of the class header

<u>Discussion</u>

Interface inheritance occurs whenever accessible members (non-private members)
of a base class are inherited by a derived class and remain accessible.

Interface inheritance is generally expressed by public inheritance,
though it can also be through protected inheritance.  Both enable polymorphic
behavior, allowing users to substitute any derived class for the base class.

Where interface inheritance occurs, every derived class must provide the same
behavior and guarantees as the base class.  This property is known as
"type consistency".

Class hierarchies that abide by the Liskov Substitution Principle (LSP) maintain
type consistency.  The LSP states:

- Preconditions cannot be strengthened in a subtype.
- Postconditions cannot be weakened in a subtype.
- Invariants of the supertype must be preserved in a subtype.

The preconditions, postconditions, and invariants make up the class contract.
Runtime checks for violations of the contract (assertions) should be added to
detect or protect against errors.

The class contract represents low-level software requirements, which the methods
of the class are traced to by being proximal to the source code.  In addition,
the methods of the derived class are traced to the requirements of the base
class (along with any requirements added by the derived class).  The derived
class must meet the requirements of the base class in order to maintain type
consistency.

Type consistency can be easier to verify when interface and implementation
inheritance are not combined.  Verification of interface inheritance is
straightforward with abstract base classes that contain only pure-virtual
function declarations and no member variables.  [^Sutter36]  Because the
abstract base class defines only interface requirements, there is no way for
derived classes to put the base class into an undesired state.  See
[this example](guidelines_inheritance_examples.md#AbstractBaseClass) for more
information.

When mixing interface and implementation inheritance is necessary, consider
using the non-virtual interface idiom (NVI), which is, in summary:

- Prefer to make interfaces non-virtual, using the Template Method pattern.
- Prefer to make virtual functions private.
- Only if derived classes need to invoke the base implementation of a virtual
  function, make the virtual function protected.
- A base class destructor should be either public and virtual, or protected
  and non-virtual.

Dispatching calls through the non-virtual interface of the base class
to the virtual methods of the derived classes provides a single place to
specify and/or check preconditions and postconditions of the virtual methods.
It also moves the dynamic dispatch from the calling routine into the base
class method.  This allows the base class and its derived class hierarchy
to be tested as a self-contained unit.

A base class destructor is a special case, because it is undefined behavior to
`delete` a derived class object using a pointer to its base class, unless the
base class destructor is virtual.  To avoid the undefined behavior, the base
class destructor should be either public and virtual (to call the appropriate
derived class constructor upon deletion), or protected and non-virtual (to
disallow deletion of a derived class).

Another alternative to virtual interface inheritance and runtime substitution
is the Curiously Recurring Template Parameter (CRTP) idiom.
The CRTP allows a non-virtual interface to be defined that is implemented by a
class that is substituted at compile-time, resulting in static dispatch and
opportunities to catch errors during compilation.

The `override` keyword should be used when overriding virtual methods.  `final`
should be used for leaf classes in a class hierarchy.  Class methods should be
marked ether `final` or `override`, but not both.

Virtual methods should not be called inside a constructor or destructor.

**Multiple Inheritance**

Use multiple inheritance only when implementing multiple interfaces (abstract
base classes with no member data or function implementations) in a single class.

When used in this way, there is no need to use the `virtual` keyword to resolve
multiple inheritance ambiguities.

**Parametric Polymorphism (Templates and Macros)**

<u>Summary</u>

- Use templates judiciously, to enhance readability, reduce code duplication,
  and provide quality abstractions
- Keep the verification activities in mind when deciding to use templates
- Understand the template instantiations that the compiler generates
- Avoid complex metaprograms and deeply nested templates
- Define preconditions, postconditions, and invariants as low-level
  requirements for all template classes and their member functions
- Avoid mixing templates with interface inheritance and overloading

<u>Discussion</u>

Use of templates (or analogous generic code) is encouraged where it reduces code
duplication or cognitive load for future programmers, or otherwise improves
the design.  Use of complex template metaprogramming is generally discouraged,
primarily because every unique template instantiation needs to be verified,
and it may be difficult to isolate intermediate template instantiations in a
metaprogram.

Every member function of a templated class must be verified for each unique
instantiation of the class.  Similarly, every template function (including
template member functions of non-template classes) must be verified for each
unique instantiation of the function.

It is possible for the compiler to successfully instantiate a template using a
type that does not meet the constraints placed on the template parameters.  The
requirements for a template class will have to describe constraints on the
types of classes it can handle, in order to perform a complete verification of
the template.  See the for more information
about the verification activities for template functions and classes.

Note that uncalled member functions in explicitly instantiated template classes
is considered deactivated code.  This code must be verified even though it is
not used to fulfill a high-level requirement.

Avoid defining template and non-template function overloads for the same
function, as this increases the burden of verifying correct control coupling.
Template specializations should generally be used instead.

```c++
template< typename T >
T foo(T x)       // primary function template
{
    return 2*x;
}

template<>
int foo(int x)   // template function specialization
{
    return 3*x;
}

int foo(int x)   // function overload
{
    return 4*x;
}

int main()
{
    unsigned x = 10;
    int y = foo(x);    // convert x to int or convert return value to int?
    return y;          // y = ??
}
```

Note that C++14 also allows generic lambdas, where the `auto` keyword is used in
the parameter list for one or more parameter types.  These should be treated as
templates, with every unique instantiation of the lambda tested. Function-like
macros may also need to be treated like templates for the purposes of
verifying type consistency.

The `nm` tool can be used to extract a list of symbols from a compiled
executable.  This can be useful for determining the template instantiations that
were generated by the compiler.

**Overloading**

<u>Summary</u>

- Overloading should be used to provide similar semantics, especially operators
- Avoid overloading an inherited base class member function in a derived class
- Avoid mixing template functions and overloads, prefer template specialization
- Be aware of the C++ implicit conversions and argument dependent lookup rules
- Be careful when mixing dynamic dispatch and function overloads

<u>Discussion</u>

Overloading is the ability to declare multiple versions (overloads) of a
function using the same name and the same number of parameters, but with
different types of parameters.  The compiler then chooses which version of the
function to use at each call site based on the types of arguments passed to
the function.

Typically, overloaded functions are used to represent the same operation but
with different types.  Prime examples are arithmetic operator overloads, which
allow user-defined types to be used in mathematical expressions along with
built-in numeric types.  This added abstraction can make it very easy to read
and comprehend code, but only if the overloaded function behaves similarly for
all the types it is overloaded for.  If the behavior of a certain overload is
different from that of other overloads, then the abstraction is broken.

Unlike the dynamic dispatch that is used to select a virtual function at
runtime, a function overload is always resolved at compile time.  This
simplifies the control flow, as there is only one implementation that
will ever be called at each call site.  In some cases, however, which
implementation the compiler selected may not be obvious just by reading the
calling code.  This is especially true when overloading is mixed with other
features including namespaces, templates, type conversion, and inheritance.

One particularly confusing aspect of overload resolution is that overloading a
single base class method in a derived class method hides all of the base class
overloads. This may not be apparent to someone reviewing your
code, and should be avoided where possible by choosing a different name for the
derived class function.

```c++
struct B
{
  double foo(int x) { return 2*x; }
  double foo(double x) { return 3*x; }
};

struct D : B
{
  double foo(int x) { return 4*x; }
};

int main()
{
  D d{};
  double x = 1.0;
  return d.foo(x);  // B::foo(double) or D::foo(int) ??
}
```

In general, when adding function overloads be aware of how the process works and 

try
to avoid creating overloads that could be confusing to future readers of your code.

**Type Conversion**

<u>Summary</u>

- Use `explicit` for all single argument constructors or type conversion
  functions, or provide rationale for allowing implicit type conversion
- Use `{}` variable initialization to avoid implicit or narrowing conversions
- Use signed `int` to represent quantities, especially for values used in
  mathematical expressions

<u>Discussion</u>

When evaluating an expression, the C++ compiler does strong type checking to
ensure that the necessary functions exist for the types of objects involved.

In certain situations, if the compiler can't find an exact type match, it
will perform implicit type conversions on the arguments to find a match.
Usually this is desirable as it improves readability and reusability, for
example, when promoting to a wider integer type or converting an int to a
double.  It can be problematic, however, when an unintended conversion occurs
and a function is called with different types than it was designed to work with.

Signed/unsigned integer conversions are a common source of errors.  Avoid mixing
signed and unsigned integers, in particular by using signed `int` to represent
quantities, even if they should only be non-negative.   Most meaningful
quantities can be represented by a 32-bit signed `int`, and while unsigned
wrapping is well-defined often it is not the intended result.  Reserve
`unsigned` integers for cases where well-defined modulo wrapping behavior is
needed or to express bit patterns (e.g., bitmasks).  Use assertions to document
that an `int` is non-negative.

In addition to the built-in rules for numeric type conversions, C++ allows
classes to provide conversion operators for converting objects to another type.
By default, these user-defined conversion operators are considered for all
possible conversions, as are any single-argument constructors.  Those functions
can be restricted from implicit conversions by declaring them with `explicit`.

In order to avoid unintentional implicit conversions, all single-argument
constructors or user-defined type conversion functions should be declared
with the `explicit` keyword. If the conversions are intended to
participate in implicit conversions, then the rationale for not declaring the
function `explicit` should be included in the routine header comments.

```c++
struct Point
{
   Point(int x, int y) : x_{x}, y_{y} {}
   int x_;
   int y_;
};
struct Implicit
{
    Implicit(int x, int y) : x_{x}, y_{y} {}
    Implicit(Point p) : x_{p.x_}, y_{p.y_} {}
    operator Point() { return Point{x_, y_}; }
    int x_;
    int y_;
};
struct Explicit
{
    explicit Explicit(int x, int y) : x_{x}, y_{y} {}
    explicit Explicit(Point p) : x_{p.x_}, y_{p.y_} {}
    explicit operator Point() { return Point{x_, y_}; }
    int x_;
    int y_;
};
void foo_implicit(Implicit) {}
void foo_explicit(Explicit) {}
void bar_implicit(Point) {}
void bar_explicit(Point) {}
int main()
{
    int x = 1; int y = 2;
    Point p{x, y};

    // no error, implicit call to converting constructor
    foo_implicit(p);

    // error: no matching function for call to 'foo_implicit'
    foo_implicit(x, y);

    // error: no matching function for call to 'foo_explicit'
    foo_explicit(p);

    // no error, explicit call to converting constructor
    foo_explicit(Explicit{p});

    // error: no matching function for call to 'bar_implicit'
    bar_implicit(x, y);

    // no error, implicit conversion to Point
    bar_implicit(Implicit(x, y));

    // error: no matching function for call to 'bar_explicit'
    bar_explicit(Explicit(x, y));

    // no error, explicit conversion to Point
    bar_explicit(static_cast< Point >(Explicit(x, y)));

    int a = 1; int b = 2;

    // no error
    Implicit i1{a, b};

    // no error
    Explicit e1{a, b};

    // no error
    Implicit i = {a, b};

    // error: chosen constructor is explicit in copy-initialization
    Explicit e = {a, b};

    // no error
    Explicit e = Explicit{a, b};

    // no error
    Explicit e = Explicit{{a, b}};
}
```

Another way to avoid unintentional implicit or narrowing conversions is to
initialize variables using curly braces instead of parentheses.  This helps
expose unintended narrowing conversions of built-in types in some cases, but
not all.

```c++
int32_t foo()
{
    return 200;
}
int8_t bar(int8_t x)
{
    return x + 100;  // undefined behavior if x > 27
}
int main()
{
    int32_t a = 100000;
    int8_t  b{a};        // error: narrowing conversion of 'a' inside { }
    int8_t  b = {a};     // error: narrowing conversion of 'a' inside { }
    int8_t  b = a;       // silent narrowing of a

    int32_t x = 10000000000;    // warning: implicit conversion changes value
    int32_t x = {10000000000};  // error: constant expression cannot be narrowed

    int32_t y = foo();
    int32_t z = bar(y);       // silent narrowing of y inside bar()
    return z;                 // returns ??
}
```

Brace initializers should also be used to avoid narrowing conversions in
constructor initializer lists and when calling an object's constructor.

```c++
struct Point
{
   Point(int x, int y) : x_{x}, y_{y} {}
   int x_;
   int y_;
};
int main()
{
    double x = 1.25; double y = 2.25;

    // no error, despite narrowing double to int
    Point p1(x, y);

    // error: non-constant-expression cannot be narrowed in initializer list
    Point p2{x, y};

    // no error, despite narrowing double to int
    Point p3 = Point(x, y);

    // error: type 'double' cannot be narrowed to 'int' in initializer list
    Point p4 = Point{x, y};
}
```