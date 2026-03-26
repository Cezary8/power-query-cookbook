# taTable build on code
Table in query created based on code

## Snippet
based on #table
```
= #table(
        {"Refresh date", "Refresh time"},
        {
            { DateTime.Date(DateTime.LocalNow()), DateTime.Time(DateTime.LocalNow()) }
        }
    )
```

```
= #table(
            type table[lower = Int64.Type, upper = Int64.Type, range name = Text.Type, _order = Int64.Type],
            {
                {999999999, null, "First/Additional/Out of Serviced/Same", 16},
                {99999999999, null, "TBC", 15}
            }
    )
```

based on formula
```
= Table.FromList(
        {Date.From( DateTime.LocalNow())},
        Splitter.SplitByNothing(),
        type table[Refresh date = Date.Type]//, null, ExtraValues.Error
    )
```
