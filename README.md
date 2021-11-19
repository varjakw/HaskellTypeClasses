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

We can extend the expression type "Expr" further to allow expressions with local variable declarations:  

```
data Expr = Val Double  
                | Add Expr Expr  
                | Mul Expr Expr  
                | Sub Expr Expr  
                | Dvd Expr Expr  
                | Var Id   
                | Def Id Expr Expr 
```  

The intended meaning of ```Def x e1 e2``` is that ```x``` is in scope in ```e2``` but not in ```e1```: compute value of ```e1``` and assign value to ```x```; then evaluate ```e2``` as overall result.  

# Dict-based Evaluation  
For the non-identifier parts of expressions, we simply pass the Dict around, but otherwise ignore it.  

```
eval :: Dict Id Double -> Expr -> Double  
eval d (Val v) = v  
eval d (Add e1 e2) = (eval d e1) + (eval d e2)  
//Mul, Sub etc done similarly
```  
Given a variable, we simply look it up:  

```
eval d (Var n) = fromJust (find d n)  
fromJusr (Just x) = x  
```

Given a ```Def```, we:  
```diff
- 1. evaluate the first expression in the given Dict  
+ 2. add a binding from the defined variable to the resulting value  
! 3. evaluate the second expression with the updated Dict  
```  
```  
eval d (Def x e1 e2) = eval(define d x (eval d e1) ) e2  
```  
where  
```diff  
- 1. (eval d e1)  
+ 2. define d x (     )  
! 3. eval (      ) e2
```



                



