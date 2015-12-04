---
title: First day on Meeting C++ 2015
date: 2015-12-04
categories: c++
---

# Meeting C++ 2015

I'm attending this year Meeting C++, these are my personal notes.

## Keynote - Understanding compiler optimization

 by Chandler Carruth

 * [SSA representation][20]
 * Simple canonical patterns
 * Inline is the single most important thing in compiler optimization
 * Context is the key for evaluate complexity

## How I stopped worrying and love metaprogramming

 by Edouard Alligand

Uses in [quasardb][7]: checks, constant generation, parser generator, protocol
generator, serializer generator.

Drawbacks: compilation time, code complexity, compiler support, skills.

[Brigand][1] Lightweight C++11 meta-programming library library.

Top 5 game changers:

 1. variadic templates
 1. `decltype`
 1. alias templates
 1. better type traits
 1. improved compilers

Useful: Command pattern (revisited)

struct <=> JSON (serialization) at compile time!

Boost.Fusion Compile time introspection:

{% highlight c++ %}

BOOST_FUSION_ADAPT_STRUCT(my_struct, (int, alpha)(std::string, omega))

{% endhighlight %}

Conclusion:

 - The good: Reliability, Performance, "Scalability".
 - The bad: need modern toolchain, time investment, complexity trap.
 - Toolbox:
   + clang 3.5+
   + [`<type_traits>`][2]
   + [`<tuple>`][3]
   + [Boost.MPL][4]
   + [Boost.Fusion][5]
   + [Boost.Hana][6]
   + [Brigand][1]

## The ways to avoid complexity in modern C++

 by [Victor Laskin][16]

> Simplicity is complexity resolved.
> [*Constantin Brancusi*][8] (romanian sculptor)

Simple rules:

 1. User C++11 for loop
 1. RAII and smart pointers
 1. Use `const` (Immutable data paradigm from functional programming)
 1. Use `auto` (return type detection C++14 and polymorphic lambdas)
 1. Use lambdas and `std::function` (messaging architectures and distributed
    systems: place code where it should be located, not where you forced to
    write it)

Why Functional programming? Because is simple: pure functions, immutable state
math like processing (improves readability), easy to build concurrent and
distributed system. C++11/C++14 its functional enough.

With immutable data there is no problem with inconsistent data. Possible
implementation using [true consts and JSON serialization][9]:

{% highlight c++ linenos %}

class EventData : public IImmutable
{
  public:
    const int id;
    const std::string title;
    const double rating;

    SERIALIZE_JSON(EventData, id, title, rating);
};

using Event = EventData::Ptr

{% endhighlight %}

Data processing: filter, map, reduce are simple blocks of functionality as
utility functors using C++11 generate compact code that are easy to write
and easy to read.

Change way of thinking: `y = f(a, b)` as C an accumulator of parts: `f`,
`a`, `b` that comes in any order.
Change way of thinking: segregation: data, iteration, action.
Change way of thinking: functional composition.

Take a look to: [Piping][10] and [Curring][11] in C++11.

Simple custom blocks: isolation of iteration code from actual action.

Functional factorization as high order functions: `ComplexF() = f1(f2(f3()))`.

[Aspect Oriented programming][12]: extract security, synchronization,
persistence, logging...

Functional chains: different functional composition: combine functions into
chains

[Transducers][13]: Take a look at slides to see transducers usage.
Take a look at [Clojure][21] documentation about [Transducers][22].

Layered architecture:

 - upper business logic layer
 - aspects layer
 - utility layer

[Real life application][14]

[C++14 cross-platform engine][15]

## An introduction to resumable functions

 by [James McNellis][17]

Resumable functions aka C++ Coroutines.

A coroutine is a generalization of a subroutine that can suspend and resume
execution. A function is a coroutine if uses `co_await`, `co_return`...

Take a look at slides, full of code of coroutines implementation details.

[N4402][18] and [P0057R1][19] paper and proposals.

## Conclusion

I enjoyed all the talks, waiting for tomorrow...

[1]: https://github.com/edouarda/brigand
[2]: http://en.cppreference.com/w/cpp/header/type_traits
[3]: http://en.cppreference.com/w/cpp/utility/tuple
[4]: http://www.boost.org/doc/libs/1_59_0/libs/mpl/doc/index.html
[5]: http://www.boost.org/doc/libs/1_59_0/libs/fusion/doc/html/index.html
[6]: http://boostorg.github.io/hana/
[7]: https://www.quasardb.net/
[8]: http://neuegrotesque.tumblr.com/image/72816273718
[9]: http://vitiy.info/immutable-data-and-serialization-in-cpp11
[10]: http://vitiy.info/functional-pipeline-in-c11/
[11]: http://vitiy.info/templates-as-first-class-citizens-in-cpp11/
[12]: http://vitiy.info/c11-functional-decomposition-easy-way-to-do-aop/
[13]: https://www.youtube.com/watch?v=vohGJjGxtJQ
[14]: http://iqoption.com/land/iqoption4-features/en
[15]: http://vitiy.info/small-presentation-of-my-cross-platform-engine-for-mobile-and-desktop-applications/
[16]: http://vitiy.info
[17]: http://www.jamesmcnellis.com
[18]: https://isocpp.org/files/papers/N4402.pdf
[19]: http://open-std.org/JTC1/SC22/WG21/docs/papers/2015/p0057r1.pdf
[20]: https://en.wikipedia.org/wiki/Static_single_assignment_form
[21]: https://en.wikipedia.org/wiki/Clojure
[22]: http://clojure.org/transducers
