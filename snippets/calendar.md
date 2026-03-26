# Calendar
Calendar based on fixed start date and max date created on maximum fact date

## Full query code
```
let
    Source = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("i45WMjIwtNQ1MAQipdhYAA==", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [min_date = _t]),
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"min_date", type date}}),
    #"Added Custom" = Table.AddColumn(#"Changed Type", "max_date", each #date(
    Date.Year(List.Max(#"f logs"[log_dt], type nullable date)),
    12,
    31
), type date),
    #"Added Custom1" = Table.AddColumn(#"Added Custom", "date", each {Number.From([min_date]).. Number.From([max_date])}),
    #"Expanded Custom" = Table.ExpandListColumn(#"Added Custom1", "date"),
    #"Removed Columns" = Table.RemoveColumns(#"Expanded Custom",{"min_date", "max_date"}),
    #"Changed Type1" = Table.TransformColumnTypes(#"Removed Columns",{{"date", type date}}),
    #"Inserted Day" = Table.AddColumn(#"Changed Type1", "day", each Date.Day([date]), Int64.Type),
    #"Inserted Month" = Table.AddColumn(#"Inserted Day", "month", each Date.Month([date]), Int64.Type),
    #"Inserted Quarter" = Table.AddColumn(#"Inserted Month", "quarter", each Date.QuarterOfYear([date]), Int64.Type),
    #"Inserted Year" = Table.AddColumn(#"Inserted Quarter", "year", each Date.Year([date]), Int64.Type)
in
    #"Inserted Year"
```
