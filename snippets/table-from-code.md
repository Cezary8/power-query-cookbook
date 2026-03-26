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
