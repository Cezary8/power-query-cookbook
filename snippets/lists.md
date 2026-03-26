# Lists

## Union all
```
List.Combine({{"<inactive>"}, #"d ad_users0"[department]})
```

## Removing list's duplicates

```
= List.Distinct(Source)
```

## Union and filtering list out on the fly
```
    Source =
    List.Union(
        {
            {"City", "West End"},
            List.Select(
                List.Distinct(Utils_Submarket[Market]),
                each (_ <> "City" and _ <> "West End")
                )
    }
```
