```@meta
DocTestSetup = quote
    using Unitful
end
```
# Unitful.jl

A Julia package for physical units. Available
[here](https://github.com/ajkeller34/Unitful.jl). Inspired by:

- [SIUnits.jl](https://github.com/keno/SIUnits.jl)
- [EngUnits.jl](https://github.com/dhoegh/EngUnits.jl)
- [Units.jl](https://github.com/timholy/Units.jl)

We want to support not only SI units but also any other unit system. We also
want to minimize or in some cases eliminate the run-time penalty of units.
There should be facilities for dimensional analysis. All of this should
integrate easily with the usual mathematical operations and collections
that are found in Julia base.

## Quick start

### Installation

+ This package requires Julia 0.5. Older versions will not be supported.
+ `Pkg.clone("https://github.com/ajkeller34/Unitful.jl.git")`
+ `Pkg.build("Unitful")`

### In Julia

First load the package:

```jl
using Unitful
```

If you encounter errors you may want to try `Pkg.build("Unitful")`.

In `deps/Defaults.jl` of the package directory, you see what is defined by
default. Feel free to edit this file to suit your needs. The Unitful package
will need to be reloaded for changes to take place.

Here is a summary of the defaults:

- SI units and their power-of-ten prefixes are defined.
- Some other units (imperial units) are defined, without power-of-ten prefixes.
- Dimensions are also defined.

Some unit abbreviations conflict with other definitions or syntax:

- `inch` is used instead of `in`, since `in` conflicts with Julia syntax
- `minute` is used instead of `min`, since `min` is a commonly used function
- `hr` is used instead of `h`, since `h` is revered as the Planck constant

### Usage examples

Units, dimensions, and fundamental constants are not exported from Unitful.
This is to avoid proliferating symbols in your namespace unnecessarily. You can
retrieve them using the [`@u_str`](@ref) string macro for convenience, or
import them from the `Unitful` package to bring them into the namespace.

```@meta
DocTestSetup = quote
    using Unitful
    °C = Unitful.°C
    °F = Unitful.°F
    μm = Unitful.μm
    m = Unitful.m
    hr = Unitful.hr
    minute = Unitful.minute
    s = Unitful.s
end
```

```jldoctest
julia> 1u"kg" == 1000u"g"             # Equivalence implies unit conversion
true

julia> !(1u"kg" === 1000u"g")         # ...and yet we can distinguish these...
true

julia> 1u"kg" === 1u"kg"              # ...and these are indistinguishable.
true
```

In the next examples we assume we have brought some units into our namespace,
e.g. using `m = u"m"`, etc.

```jldoctest
julia> uconvert(°C, 212°F)
100//1 °C

julia> uconvert(μm/(m*°F), 9μm/(m*°C))
5//1 °F^-1 μm m^-1

julia> mod(1hr+3minute+5s, 24s)
17//1 s
```

See `test/runtests.jl` for more usage examples.

## To do

- Clean up how units/quantities are displayed, especially with regards to how
  their *types* are displayed. How to interpret what is printed is confusing.

- Benchmarking needed.

- More tests are always appreciated and necessary.

- Add support for uncertainties? For quantities with uncertainty, `isapprox`
  becomes a loaded / ambiguous name.

- Specialized exceptions for dimensional mismatches, other unit-related troubles?