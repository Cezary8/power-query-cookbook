# Custom replacing
Idea is to process replacement not on whole column but only on this items that meet condition.
In practice, this technique is used to modify conditionally data without creating another column

## Snippet
```
= Table.ReplaceValue(#"Added _rank_division",
            each [_rank_division],
            each if [division] = "<inactive>" then 9998
            else if [division] = "<undefined>" then 9999
            else [Index] ,
            Replacer.ReplaceValue,{"_rank_division"}
            )
```
