# Lambda expression

```text
[ capture clause ] (parameters) -> return-type  
{   
   definition of method   
} 
```

return-type can be ignored, but **not** in some complex case.

We can capture external variables from enclosing scope by three ways :  
      \[&\] : capture all external variable by reference  
      \[=\] : capture all external variable by value  
      \[a, &b\] : capture a by value and b by reference

