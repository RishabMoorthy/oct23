# StubServer v2: Placeholder Syntax (Current vs Proposed)

Date: 2026-10-23
Status: Proposal, awaiting team confirmation

## Why change

Placeholders in StubServer v2 today mix ReadyAPI-style magic strings (`${#MockResponse#Request#...}`, `${#MockService#...}`) with a bare `${name}` form. A bare name doesn't say where the value comes from: a data generator, a property, a filter, a path parameter or a script variable. Because they all share one store, two sources with the same name overwrite each other.

The proposal uses one pattern everywhere, specific to StubServer:

```
${stub.<source>.<name>}
```

It reads the same in a response body, a Database SQL query and an Excel column mapping.

Values in `<angle brackets>` are what the user fills in.

## Placeholder mapping

| What | Current | Syntax type | Proposed |
|---|---|---|---|
| Magic string, JSON request | `${#MockResponse#Request#$['body']['id']}` | `${#MockResponse#Request#<jsonPath>}` | `${stub.request.<field.path>}` |
| Magic string, XML request | `${#MockResponse#Request#//Order[1]/Id[1]}` | `${#MockResponse#Request#<xPath>}` | `${stub.request.<xPath>}` |
| Request body field (SQL/Excel) | `${request.customer.id}` | `${request.<field.path>}` | `${stub.request.<field.path>}` |
| Path parameter (response) | `${id}` | `${<pathParamName>}` | `${stub.path.<pathParamName>}` |
| Path parameter (SQL/Excel) | `${path.id}` | `${path.<pathParamName>}` | `${stub.path.<pathParamName>}` |
| Query parameter (SQL/Excel) | `${query.region}` | `${query.<queryParamName>}` | `${stub.query.<queryParamName>}` |
| Request header | not supported | n/a | `${stub.header.<headerName>}` |
| Filter value (response) | `${custId}` | `${<filterName>}` | `${stub.filter.<filterName>}` |
| Filter value (SQL/Excel) | `${filter.custId}` / `${filters.custId}` | `${filter.<filterName>}` / `${filters.<filterName>}` | `${stub.filter.<filterName>}` |
| Local / VS property | `${env}` | `${<propertyKey>}` | `${stub.prop.<propertyKey>}` |
| Data generator | `${SequentialNumber}` | `${<generatorVariable>}` | `${stub.gen.<generatorVariable>}` |
| Data source column | `${DB_Two.USER_NAME}` | `${<dataSourceName>.<columnName>}` | `${stub.data.<dataSourceName>.<columnName>}` |
| Data source row count | `${DB_Two.rowCount}` | `${<dataSourceName>.rowCount}` | `${stub.data.<dataSourceName>.rowCount}` |
| Mock service property | `${#MockService#msisdn}` | `${#MockService#<propertyName>}` | `${stub.service.<propertyName>}` |
| Mock service property, `#$` form | `${#MockService#msisdn#$}` | `${#MockService#<propertyName>#$}` | `${stub.service.<propertyName>}` |
| Global property | `${#Global#token}` (blanked today, see Known issues) | `${#Global#<propertyName>}` | `${stub.global.<propertyName>}` |
| Script variable (`context.x = ...`) | `${orderId}` | `${<variableName>}` | `${stub.var.<variableName>}` |
| Current date/time | `${=new java.text.SimpleDateFormat("yyyy-MM-dd").format(new java.util.Date())}` | `${=new java.text.SimpleDateFormat("<format>").format(new java.util.Date())}` | `${stub.now:<format>}` |

## Unchanged (not placeholders)

| What | Current | Syntax type | Proposed |
|---|---|---|---|
| Excel MappingType | `Path` / `Request` / `Query String` / `Filters` | `<MappingType>` plus a parameter name | Same, or the parameter written as `${stub.<source>.<name>}` |
| Save Property Value to File | `Filter` / `Properties` / `DataGenerator` / `Stub Request` / `GlobalProperties` | `<PropertyType>` plus `<PropertyName>` | Same |
| Groovy script | `context.DB_Two.USER_NAME`, `globalProperties`, `mockService` | `context.<name>` | Same |

## Examples

Response body (JSON):

```json
{
  "orderId": "${stub.path.orderId}",
  "customer": "${stub.request.customer.name}",
  "region": "${stub.query.region}",
  "trackingNo": "${stub.gen.SequentialNumber}",
  "userName": "${stub.data.DB_Two.USER_NAME}",
  "createdOn": "${stub.now:yyyy-MM-dd'T'HH:mm:ss}"
}
```

Database data source query:

```sql
SELECT * FROM USERS WHERE ID = ${stub.path.id} AND REGION = ${stub.filter.region}
```

## Known issues in the current code

1. **Global properties never reach a response.** `${#Global#x}` is replaced with an empty string by `TemplateReplacer`. The class that handles `Global#` (`PropertyResolver` / `MockResponseProcessor`) is never called.
2. **`path.`, `query.` and `request.` only work in data sources.** In a response body they come out blank; only plain `${id}` works there.
3. **Every source shares one flat map.** Values are written in this order, and the last one wins on a name clash: path parameters, then properties, data generators, filters, data sources, scripts.
4. **Unresolved placeholders are removed silently.** A typo becomes an empty value with no warning.

The `stub.<source>` prefixes fix issues 2 and 3. Issues 1 and 4 need a small resolver change in the same piece of work.

## Compatibility

All current forms keep working, so existing `.vs` files don't need to change. New `.vs` files and the UI would use the `stub.` forms.
