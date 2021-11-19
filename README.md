# HaskellTypeClasses
This is an assignment demonstrating Type Classes in Haskell, using evaluation of expressions.

# Introduction



Defining a link between key and datum is simply constructing such a pair onto the start of the list:

```
define  ::  Dict k d -> k -> d -> Dict k d
define d s v = (s,v):d
```

Looking up a term is simply searching along the list:

```
find :: Eq k => Dict k d -> k -> Maybe d
find [] _ = Nothing
find ( (s,v) : ds) name | name == s = Just v
                        | otherwise = find ds name
```
We see here the notation "Eq k =>" is a context simply stating that the Dict class depends on the k class.

However we do need to handle the case where the key is not present. This is done with the Maybe type.  

Maybe from Data.Maybe  


```
import Data.Maybe

isJust            :: Maybe a -> Bool
isJust (Just a)   = True
isJust Nothing    = False

isNothing         :: Maybe a -> Bool
isNothing         = not . isJust

fromJust          :: Maybe a -> a
fromJust (Just a) = a
fromJust Nothing  = error "Maybe.fromJust: Nothing"

fromMaybe         :: a -> Maybe a -> a
fromMaybe d Nothing = d
fromMaybe d (Just a) = a
```  



