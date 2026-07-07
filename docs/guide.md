# Research‑Oriented Technology Stack Guide

## Preface

This document is a practical subset of a five‑stage research‑style methodology:

| Meta‑Guide      | Tech‑Stack‑Guide |
|-----------------|------------------|
| Problem Space   | *(omitted)*      |
| Abstraction     | *(omitted)*      |
| Modeling        | Modeling         |
| Prototype       | Prototype        |
| Product         | Product          |

The **meta‑guide** covers conceptual stages such as *Problem Space* and *Abstraction*, which serve primarily as thinking frameworks rather than programming practices.

The **tech‑stack‑guide**, by contrast, focuses only on the engineering‑related stages (*Modeling → Prototype → Product*), where tools, languages, and workflows directly apply.

The additional *Modeling* stage plays a crucial role: it filters out unsuitable problems before effort is invested in prototyping. This helps prevent wasted development on projects that are conceptually unsound.

Technology choices are inevitably shaped by personal experience and team preferences. Therefore, this guide should be used as a reference point, to be adapted according to actual needs.

The document first outlines the overall principles for technology selection, followed by an analysis of the advantages and limitations of each stack.

## Technology Selection

### Modeling Stage

* Begin with free text to capture initial ideas and inspirations.
* Use **OCaml** for modeling logical relationships.
* Use **Julia** for modeling mathematical computations.
* If a model requires both logical relationships and mathematical computations, choose either OCaml or Julia depending on team preference.
* For mathematical operations within OCaml, consider using the `Lwt` library.
* For logical relationship checks within Julia, consider using the `MLStyle` library.

### Prototype Stage

* **Workflow Projects**: Use POSIX sh.
* **Other Project Types**: Use one of Perl, Python, Ruby or Julia.

### Production Stage

* Use either OCaml, F# or Rust.
* If the team is not familiar with ML, use either Java or C#.

### Specific Deployment Environments

* **Browser and Edge Function Projects**: Use JavaScript.
* **Content Website Projects**: Use PHP.
* **Enterprise Projects**: Use Java or C#.
* **Mobile Projects**: Use Dart.

> **Note**: Deployment stacks should only be considered when constrained by specific deployment targets. Since deployment stacks often have inherent limitations or drawbacks, they are not used as prototype or production stacks.

### Legacy Projects

* Maintain legacy projects in their original language. Do not rewrite unless there is a clear reason.
* If a technology stack stops receiving updates or is removed from the system, migrate the project as soon as possible.
* If the deployment environment is deprecated and the original stack can no longer be used, migrate the project whenever feasible.
* If performance bottlenecks persist despite correct data structures and algorithms, consider migrating the project.

## Prototype

* Perl, Python and Ruby can all serve as prototype stacks, with no significant difference in core language capabilities or development efficiency.
* It is recommended to start with a **single script file** to quickly validate ideas, and refactor into a project structure once the scale grows.
* Since they are dynamic languages, many errors only surface at runtime, making them unsuitable as production stacks.

## Product

* The core focus of production projects is long-term stability. Therefore, choose technology stacks with slower development iterations but higher code quality and lower maintenance costs.
* OCaml, F# and Rust feature powerful compilers that perform static checks at compile time, eliminating most common errors.
* It is recommended to use community-standard project build and management tools to generate a template project, then copy this template into the main Git repository to begin development.

## Minimal Learning Environment

* When exploring a new tech stack, always name the entry file `program` or `Program`, then choose the appropriate extension based on the stack.
* For practicing language features, consistently use the same entry file to avoid the overhead of setting up projects.
* Accelerate the learning process by leveraging template projects, compilation scripts, and artifact‑cleaning tools.
* Preserve valuable sample code in private GitHub repositories; the entry file itself does not need to be retained.
* Only when progressing to building applications or libraries should you dive into the stack’s project management tools.

## OCaml

* Built-in efficient garbage collection, so developers do not need to manually manage memory lifecycles.
* Supports functional programming syntax features and type inference.
* Highly expressive syntax design allows complex logic to be implemented with concise code.
* When modeling with OCaml, as long as the type hierarchy is correct and the code compiles, that’s sufficient — you don’t need a complete process.
* Functions can be written as stubs without full implementation.
* Take advantage of OCaml’s Variants and Pattern Matching to build type hierarchies.

## Julia

* Uses JIT compilation via LLVM and automatically calls high‑performance numerical libraries such as BLAS, combining characteristics of both prototyping and production.
* Employs gradual type annotations, allowing projects to start without explicit types and add them later when transitioning to production.
* Replaces traditional object systems with structures and multiple dispatch.
* Writing computational processes as functions provides useful references for future implementation.
* A small set of literal values can serve as representative data; raw datasets are not required.

## POSIX sh

* POSIX sh is the fastest technology stack for building workflow projects, but it is not suitable for other types of project development.
* Initially, it is recommended to start with a **single script file**. As the program grows in scale and the logic becomes more complex, refactor it into a proper project structure.
* The language features are overly permissive, making it easy to write code that appears to work but hides subtle bugs. Development must be paired with static analysis tools such as `ShellCheck` for code inspection.
* Shell scripts lack the concept of libraries. For cross-project reuse, you need to maintain a set of common utility functions and share them across projects.
* When facing complex logic or special requirements, other scripting languages can be introduced as sidecars (auxiliary scripts) to assist with processing, but the overall workflow should still be controlled by shell scripts.

This snippet demonstrates how to include a library in a shell script:

```shell
# bin/cli
CWD=$(CDPATH='' cd -- "$(dirname -- "$0")" && pwd) || exit 1
. "${CWD}/../lib/utils.sh"
```

## Perl

* As part of the standard Unix/Linux setup, it is preinstalled on almost all operating systems.
* Its regex syntax and performance are the de facto industry standard.
* Supports one-liner mode, making it ideal for quick text and file stream processing.

> **Note**: Traditional Perl syntax is highly flexible and relatively harder to understand. However, since version `5.36+`, modernized syntax has significantly improved readability, which is why Perl is included as a recommended prototype stack.

## Python

* Has the largest ecosystem of community libraries, with nearly all emerging applications providing Python support first.
* Its syntax structure is simple, resembling executable pseudocode, making it highly readable and easy for team maintenance.

## Ruby

* Designed with developer experience in mind, featuring elegant and expressive syntax.
* Its regex support and performance are nearly on par with Perl.
* Friendly toward domain-specific languages (DSL), making it suitable for building internal DSL tools.

## Rust

* Through ownership and the borrow checker, Rust achieves memory safety without relying on garbage collection.
* While leaning toward low-level systems programming, it also incorporates many high-level features from functional and object-oriented paradigms.
* Offers zero-cost abstractions and extreme performance, making it the top choice for new projects replacing C or C++.

## Java / C#

- The essence of Java / C# lies in being enterprise stacks, characterized by libraries that address common problems, allowing developers to focus on core logic.
- Enterprise stacks adopt managed code, so developers don’t need to handle memory management, reducing the likelihood of program crashes.
- The compilers of Java / C# are not particularly strong in enforcing code quality, but the ecosystems are complete, making implementation straightforward.
- It is rare to encounter projects that can only be solved by one of these stacks; choosing between Java and C# is usually a matter of team preference.

## JavaScript

* As the only native language for browsers and edge computing, JavaScript still holds an irreplaceable position in these domains.
* Since many potential errors only surface at runtime, it is not suitable as a production stack.
* Developers must adapt to the peculiar syntax and behavioral quirks left by JavaScript’s history, which can cause friction when writing prototypes.
* When writing code, it is necessary to constantly consider whether the target environment is the browser or Node.js, and manually implement runtime glue for compatibility. This becomes an unnecessary burden for prototype development.
* Project initialization is relatively cumbersome, and the ecosystem’s toolchains change frequently without long-term consistent standards, adding unnecessary configuration friction.

## PHP

* PHP is essentially a scripting language developed around web output, with its core strength in dynamically generating HTML, XML, JSON, and image files.
* Its core functionality is highly focused on web templates and web infrastructure, making it still practical and efficient for building content websites.
* During prototype development, web templates and web output are redundant features, so PHP is not recommended as a prototype stack.
* As a dynamic language, many errors only surface at runtime, making it unsuitable as a production stack.

For building content websites, it is recommended to adopt a **tool-based encapsulation strategy**:

> **Practical Experience**: PHP can be used to develop a static site generator (SSG). Once developed, all subsequent content website projects can directly use this SSG without writing new PHP code. This effectively encapsulates PHP into a single tool, leveraging its strengths while avoiding repetitive development.

## F#

* Although its syntax is not fully compatible, F# effectively extends the technical lifespan of OCaml and preserves the functional programming mindset, allowing the team’s technology stack to transition smoothly.
* Enterprise projects focus on quickly leveraging existing APIs. As a mainstream enterprise technology stack, .NET provides an extensive and mature library of business functionalities.
* Since F# code execution depends on the .NET runtime environment, this introduces additional deployment costs when releasing production systems, making F# unsuitable as a production stack.

## Dart

* Dart was originally designed as a replacement for JavaScript, with a syntax structure that can be seen as a more complete and modern alternative.
* Its core ecosystem is highly centered around the Flutter framework, making it extremely suitable as a specific deployment environment for mobile projects.
* Dart enforces project-based code organization, which introduces unnecessary configuration friction during the prototype stage when ideas need to be validated quickly.
* Although its compiler includes built-in null safety, the overall improvement in code quality and architectural stability is limited, so Dart is not recommended as a production stack.

## C / C++

* Originally designed to eliminate the complexity of writing assembly language while retaining direct control over low-level systems and hardware.
* The compiler itself provides relatively little practical assistance in improving code quality, so development relies heavily on external static analysis tools to strengthen and refine code quality.
* Developers must manually allocate and free memory, which is often the primary source of system security vulnerabilities and common bugs.
* In the case of C, its built-in abstraction mechanisms are quite limited. While this grants developers significant freedom, the trade-off is the need to write boilerplate code manually.

## Go (Golang)

* Originally designed to simplify C/C++ development while providing fast compilation and cross-platform deployment capabilities.
* Widely used in cloud infrastructure (Kubernetes, Docker) and backend services.
* Language features are deliberately kept simple, with limited abstraction capabilities. Sometimes boilerplate code must be written, making it unsuitable as a prototype stack.
* Error handling mechanisms are overly verbose, often affecting code readability.
* The compiler provides limited guarantees for code quality, requiring reliance on external tools, which makes Go unsuitable as a production stack.
