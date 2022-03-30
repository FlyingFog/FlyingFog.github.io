```cs
if (child is FunctionDefinition function)
{
    if (function.IsConstructor)
    {
        context.AddConstructorToContract(node, function);
    }
    else if (function.IsFallback)
    {
        context.AddFallbackToContract(node, function);
    }
}

```

