# Managing duplicates
Mechanism simillar to 'rank over partition by' used to count multiplications

## Snippet
```
    #"Inserted Lowercased Text" = Table.AddColumn(#"Changed Type", "lowercase", each Text.Lower([department]), type text),
    #"Grouped Rows" = Table.Group(#"Inserted Lowercased Text", {"lowercase"}, {{"Details", each _, type table [department=nullable text, lowercase=text]}}),
    #"Sorted Rows" = Table.Sort(#"Grouped Rows",{{"lowercase", Order.Ascending}}),
    #"Added Index" = Table.AddIndexColumn(#"Sorted Rows", "_rank", 1, 1, Int64.Type),
    Custom1 = Table.ReplaceValue(#"Added Index",
            each [_rank],
            each if [lowercase] = "<inactive>" then 9998
            else if [lowercase] = "<undefined>" then 9999
            else [_rank] ,Replacer.ReplaceValue,{"_rank"}
            ),
    #"Changed Type1" = Table.TransformColumnTypes(Custom1,{{"_rank", Int64.Type}}),
    #"Removed Columns" = Table.RemoveColumns(#"Changed Type1",{"lowercase"}),
    #"Expanded Details" = Table.ExpandTableColumn(#"Removed Columns", "Details", {"department", "lowercase"}, {"department", "lowercase"})
```
