# Adapt

[![Build Status](https://travis-ci.org/MikeInnes/Adapt.jl.svg?branch=master)](https://travis-ci.org/MikeInnes/Adapt.jl)

The `adapt(T, x)` function acts like `convert(T, x)`, but without the restriction of returning a `T`. This allows you to "convert" wrapper types like `RowVector` to be GPU compatible (for example) without throwing away the wrapper.

e.g.

```julia
adapt(CuArray, ::RowVector{Array})::RowVector{CuArray}
```

New data types like `RowVector` should overload `adapt(T, ::RowVector)` (usually just to forward the call to `adapt`).

```julia
adapt(T, x::RowVector) = RowVector(adapt(T, x.vec))
```

New adaptor types like `CuArray` should overload `adapt_` for compatible types.

```julia
adapt_(::Type{<:CuArray}, xs::AbstractArray) =
  isbits(xs) ? xs : convert(CuArray, xs)
```
