# taTable build on code
Table in query created based on code

## Snippet
```
= #table(
        {"Refresh date", "Refresh time"},
        {
            { DateTime.Date(DateTime.LocalNow()), DateTime.Time(DateTime.LocalNow()) }
        }
    )
```

```
= Table.FromList(
        {Date.From( DateTime.LocalNow())},
        Splitter.SplitByNothing(),
        type table[Refresh date = Date.Type]//, null, ExtraValues.Error
    )
```
