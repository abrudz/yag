# Last
Last is a simple function that comes up often. As three existing idioms (`⊢/`/`⊢⌿`, `⊃⌽`/`↑⌽`, `⊃⌽,`/`↑⌽,`), it is a prime candidate for becoming a primitive.

Last is especially elegant in connection with First, e.g. for things like applying a function to a pair as arguments with `⊃f⊇` or and `{(⍺<⊃⍵)∨⍺≥⊃⌽⍵}` becomes `<∘⊃∨≥∘⊇`. Various constructs can be optimised because they get obvious spellings, like index of last 1 being `⊇⍸`. Insert into a pair: `(⊃,'=',⊇)'name' 'value'`.
