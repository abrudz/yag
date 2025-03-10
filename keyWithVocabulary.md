# Key with Vocabulary

The Key operator (`⌸`) has multiple issues where one cannot easily:

1. choose the order in which results are returned
2. choose a subset of keys for which to return results
3. include additional keys not present in the current keys
4. get the unique keys separately from the indices/data without the expensive unique computation happining twice

_Key with Vocabulary_ solves all these problems by giving Key an array operand (the vocabulary) instead of an aggregation function. Instead, `{⊂⍵}` will always be used as aggregation function. There is no loss of information as the left argument is exactly the collection of left arguments to the aggregation function, and thus, if `V` is the vocabulary, `↑V f¨ V⌸ data` is equivalent to `f⌸ data` but with the added benefit of a vocabulary. Similarly for `↑V f¨ keys V⌸ values` vs `keys f⌸ values`.

## Choose the order in which results are returned
Consider counting DNA bases:
```apl
      dna←'GTAGGGCGAGTA'
      {≢⍵}⌸dna
6 2 3 1
```
This doesn't work because we don't know the order of appearance of the bases. We'd have to sort the data (expensive) or the result (complicated):
```apl
      {≢⍵}⌸dna['AGCT'⍋dna]
3 6 1 2
      {⍵['AGCT'⍋⊣/⍵;2]}{⍺,≢⍵}⌸dna
3 6 1 2
```
Using Key with vocabulary:
```apl
      ≢¨'AGCT'Ⓚ dna
3 6 1 2
```
## Choose a subset of keys for which to return results
Consider vowel frequencies:
```apl
      text←'an elephantine hors d''oeuvre'
      {≢⍵}⌸text
4 5 5 4 2 2 1 3 4 1 3 2 2 1 1 1
```
How do we get letter counts for just the vowels? We'd have to filter the text (expensive) or the result (complicated):
```apl
      {≢⍵}⌸text∩'aeiou'
2 5 1 2 1
      {⍵[;2]/⍨⍵[;1]∊'aeiou'}{⍺,≢⍵}⌸text
2 5 1 2 1
```
Using Key with vocabulary:
```apl
      ≢¨'aeiou'⌸text
2 5 1 2 1
```
## Include additional keys not present in the current keys
Letter frequencies again:
```apl
      text2←'an elephantine appetiser'
      {≢⍵}⌸text2∩'aeiou'
3 5 2
```
How do we ensure all vowels are accounted for? We'd have to either amend the text (expensive) or the result (complicated):
```apl
      ¯1+{≢⍵}⌸text2(∩,⊢)'aeiou'
3 5 2 0 0
      {(⍵[;2],0)[⍵[;1]⍳'aeiou']}{⍺,≢⍵}⌸text
3 5 2 0 0
```
Using Key with vocabulary:
```apl
      ≢¨'aeiou'⌸text
3 5 2 0 0
```
