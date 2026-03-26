# Managing duplicates
Mechanism simillar to 'rank over partition by' used to count multiplications

## Snippet
```
let
    Source = Csv.Document(File.Contents("D:\praca\Savills\users.csv"),[Delimiter=",", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.None]),
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Column1", type text}, {"Column2", type text}, {"Column3", type text}, {"Column4", type text}, {"Column5", type text}}),
    #"Promoted Headers" = Table.PromoteHeaders(#"Changed Type", [PromoteAllScalars=true]),
    #"Changed Type1" = Table.TransformColumnTypes(#"Promoted Headers",{{"display_name", type text}, {"email_address", type text}, {"country", type text}, {"office_location", type text}, {"department", type text}}),
    #"Sorted Rows" = Table.Sort(#"Changed Type1",{{"email_address", Order.Ascending}, {"country", Order.Descending}}),
    #"Grouped Rows" = Table.Group(#"Sorted Rows", {"email_address"}, {{"Count", each _, type table [display_name=nullable text, email_address=nullable text, country=nullable text, office_location=nullable text, department=nullable text]}}),
    #"Added Custom" = Table.AddColumn(#"Grouped Rows", "Custom", each Table.AddIndexColumn([Count],"Rank",1,1)),
    #"Removed Other Columns" = Table.SelectColumns(#"Added Custom",{"Custom"}),
    #"Expanded Custom" = Table.ExpandTableColumn(#"Removed Other Columns", "Custom", {"display_name", "email_address", "country", "office_location", "department", "Rank"}, {"display_name", "email_address", "country", "office_location", "department", "Rank"}),
    #"Changed Type2" = Table.TransformColumnTypes(#"Expanded Custom",{{"display_name", type text}, {"email_address", type text}, {"country", type text}, {"office_location", type text}, {"department", type text}, {"Rank", Int64.Type}}),
    #"Filtered Rows" = Table.SelectRows(#"Changed Type2", each [Rank] = 1),
    #"Removed Columns" = Table.RemoveColumns(#"Filtered Rows",{"Rank"})
in
    #"Removed Columns"
```
