# Buffer

## Query
```
let
    BuforSłówKluczowych = Table.Buffer(#"Słowa kluczowe"),
    Źródło = #"Wpisy Microsoft Press",
    #"Usunięto inne kolumny" = Table.SelectColumns(Źródło,{"ID wpisu", "Wpis"}),
    #"Zduplikowano kolumnę" = Table.DuplicateColumn(#"Usunięto inne kolumny", "Wpis", "Wpis — kopia"),
    #"Tekst pisany małymi literami" = Table.TransformColumns(#"Zduplikowano kolumnę",{{"Wpis — kopia", Text.Lower, type text}}),
    #"Zmieniono nazwy kolumn" = Table.RenameColumns(#"Tekst pisany małymi literami",{{"Wpis — kopia", "Małe litery"}}),
    #"Dodano kolumnę niestandardową" = Table.AddColumn(#"Zmieniono nazwy kolumn", "Iloczyn kartezjański", each BuforSłówKluczowych),
    #"Rozwinięty element Iloczyn kartezjański" = Table.ExpandTableColumn(#"Dodano kolumnę niestandardową", "Iloczyn kartezjański", {"Słowo kluczowe"}, {"Słowo kluczowe"}),
    #"Dodano kolumnę warunkową" = Table.AddColumn(#"Rozwinięty element Iloczyn kartezjański", "Temat", each if Text.Contains([Małe litery], [Słowo kluczowe]) then [Słowo kluczowe] else null),
    #"Przefiltrowano wiersze" = Table.SelectRows(#"Dodano kolumnę warunkową", each [Temat] <> null and [Temat] <> ""),
    #"Usunięto kolumny" = Table.RemoveColumns(#"Przefiltrowano wiersze",{"Wpis", "Małe litery", "Słowo kluczowe"})
in
    #"Usunięto kolumny"
```
