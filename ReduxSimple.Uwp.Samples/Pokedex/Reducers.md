```csharp
public static class Reducers
{
    public static IEnumerable<On<PokedexState>> CreateReducers()
    {
        return new List<On<PokedexState>>
        {
            On<GetPokemonListAction, GetPokemonByIdAction, PokedexState>(
                state => state.With(new { Loading = true })
            ),
            On<GetPokemonListFullfilledAction, PokedexState>(
                (state, action) => state.With(new {
                    Pokedex = PokedexAdapter.AddAll(action.Pokedex, state.Pokedex),
                    Loading = false,
                    Errors = ImmutableList<string>.Empty
                })
            ),
            On<GetPokemonListFailedAction, PokedexState>(
                (state, action) => state.With(new {
                    Loading = false,
                    Errors = state.Errors.Add(action.Exception.Message)
                })
            ),
            On<GetPokemonByIdFullfilledAction, PokedexState>(
                (state, action) => state.With(new {
                    Pokemon = Option<Pokemon>.Some(action.Pokemon),
                    Loading = false,
                    Errors = ImmutableList<string>.Empty
                })
            ),
            On<GetPokemonByIdFailedAction, PokedexState>(
                (state, action) => state.With(new {
                    Loading = false,
                    Errors = state.Errors.Add(action.Exception.Message)
                })
            ),
            On<UpdateSearchStringAction, PokedexState>(
                (state, action) => state.With(new {
                    action.Search
                })
            ),
            On<ResetPokemonAction, PokedexState>(
                state => state.With(new { Pokemon = Option<Pokemon>.None() })
            )
        };
    }
}
```