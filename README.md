# ShorterMaps

`~M` sigil for map shorthand. `~M{a} ~> %{a: a}`

## Motivation

Code like `%{id: id, name: name, address: address}` occurs with high
frequency in many programming languages.  In Elixir, additional uses occur as we
pattern match to destructure existing maps.

ES6 (ES2015 for those folks who insist on proper names) provided javascript with
a shorthand to create maps with keys inferred by variable names, and allowed
destructuring those maps into variables named for the keys.  `ShorterMaps`
provides that functionality to Elixir.

### Credits

ShorterMaps adds additional features to the original project, `ShortMaps`, located [here][original-repo]. The reasons for the divergence are summarized [here][divergent-opinion-issue].

The key syntactic difference is motivated by the trailing `a` in `~m{}a`.  To maintain backward compatibility, that syntax still works, but ShorterMaps adds a ~M sigil that defaults to the `a` modifier.

## Basic Usage

**Note that you always have to `import ShorterMaps` for the sigil to work.**

### Pattern Matching

```elixir
iex> import ShortMaps
...> ~M(foo bar baz) = %{foo: 1, bar: 2, baz: 3}
...> foo
1
```

### Map Construction

```elixir
iex> import ShortMaps
...> name = "Meg"
...> ~M(name) # M = atom keys
%{name: "Meg"}
...> ~m(name) # m = String keys
%{"name" => "Meg"}
```

### Structs

The first word inside the sigil must be '%' followed by the module name:

```elixir
iex> import ShortMaps
...> defmodule MyStruct do
...>   defstruct [id: 0, name: ""]
...> end
...>
...> # Struct construction:
...> id = 5
...> name = "Chris"
...> ~M{%MyStruct id name}
%MyStruct{id: 5, name: "Chris"}
...>
...> # Pattern Matching:
...> ~M{%MyStruct id} = %MyStruct{id: 1, name: "Bob"}
...> id
1
```

### Variable Pinning

```elixir
iex> import ShortMaps
...> name = "Meg"
...> ~M(^name) = %{name: "Meg"}
%{name: "Meg"}
...> ~M(^name) = %{name: "Megan"}
** (MatchError) no match of right hand side value: %{name: "Megan"}
```

You can see more examples and read some docs in the docs for the `sigil_m`
macro.

## Installation

```elixir
# mix.exs

defp deps do
  [
    {:shorter_maps, "~> 1.0"},
  ]
end
```

[google-groups]: https://groups.google.com/forum/#!topic/elixir-lang-core/NoUo2gqQR3I
[original-repo]: https://github.com/whatyouhide/short_maps
[divergent-opinion-issue]: https://github.com/whatyouhide/short_maps/issues/11
