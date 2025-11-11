---
author: wadepickett
description: Endpoint-specific OpenAPI operation transformers in ASP.NET Core 10.0
ms.author: wpickett
ms.custom: mvc
ms.date: 11/11/2025
---

### Endpoint-specific OpenAPI operation transformers

OpenAPI operation transformers can now be registered directly on individual endpoints using the `AddOpenApiOperationTransformer` extension method. This allows customization of OpenAPI metadata to be colocated with the endpoint definition, rather than requiring global configuration with path-based conditional logic.

Prior to this feature, modifying OpenAPI data for specific endpoints required registering a global operation transformer and implementing conditional logic to target specific endpoints. For example:

```csharp
builder.Services.AddOpenApi(options =>
{
    options.AddOperationTransformer((operation, context, ct) =>
    {
        if (context.Description.RelativePath == "weatherforecast")
        {
            operation.Responses["200"].Description = "Weather forecast for today";
        }
        return Task.CompletedTask;
    });
});
```

With the new `AddOpenApiOperationTransformer` API, the same customization can be applied directly to the endpoint:

:::code language="csharp" source="~/fundamentals/openapi/samples/10.x/WebMinOpenApi/Program.cs" id="snippet_operationtransformer2":::

This approach improves code readability and maintainability by keeping endpoint-specific customizations close to the endpoint definitions. The `AddOpenApiOperationTransformer` method provides access to the full <xref:Microsoft.AspNetCore.OpenApi.OpenApiOperationTransformerContext>, enabling comprehensive customization of the OpenAPI operation, including:

* Setting operation properties like `Deprecated`, `Summary`, or `Description`
* Customizing response descriptions
* Adding or modifying security requirements
* Modifying parameters or request bodies

For more information, see <xref:fundamentals/openapi/customize-openapi#use-operation-transformers>.

[Community contribution (`dotnet/aspnetcore` #59180)](https://github.com/dotnet/aspnetcore/issues/59180) by [Sander ten Brinke](https://github.com/sander1095).
