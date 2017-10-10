#Kendo.DynamicLinqCore2.0

# Note
Kendo.DynamicLinqCore2 is referred to Kendo.DynamicLinq by [kendo-labs](https://github.com/kendo-labs/dlinq-helpers). 
Related notes can refer it.

## Description
Kendo.DynamicLinqCore2 implements server paging, filtering, sorting and aggregating via Dynamic Linq for .Net Core2.0.

## Build NuGet Package
1. Open command line console
2. Switch to project root directory.(src\Kendo.DynamicLinqCore2)
3. Run "dotnet restore"
4. Run "dotnet pack --configuration release" 

## Usage
1. Add the Kendo.DynamicLinqCore2 NuGet package to your project.
2. Configure your Kendo DataSource to send its options as JSON.

        parameterMap: function(options, type) {
            return JSON.stringify(options);
        }
3. Configure the `schema` of the DataSource.

        schema: {
            data: "Data",
            total: "Total",
            aggregates: "Aggregates",
            groups: "Group"
        }
4. Import the Kendo.DynamicLinqCore namespace.
5. Use the `ToDataSourceResult` extension method to apply paging, sorting and filtering.

        [WebMethod]
        public static DataSourceResult Products(int take, int skip, IEnumerable<Sort> sort, Filter filter, IEnumerable<Aggregator> aggregates, IEnumerable<Sort> group)
        {
            using (var northwind = new Northwind())
            {
                return northwind.Products
                    .OrderBy(p => p.ProductID) // EF requires ordering for paging                    
                    .Select(p => new ProductViewModel // Use a view model to avoid serializing internal Entity Framework properties as JSON
                    {
                        ProductID = p.ProductID,
                        ProductName = p.ProductName,
                        UnitPrice = p.UnitPrice,
                        UnitsInStock = p.UnitsInStock,
                        Discontinued = p.Discontinued
                    })
                 .ToDataSourceResult(take, skip, sort, filter, aggregates, group);
            }
        }

## Other Examples
This example has full demo with .Net Core2.0 by [Vahid Nasiri](https://github.com/VahidN).

- [KendoUI.Core.Samples](https://github.com/VahidN/KendoUI.Core.Samples)
