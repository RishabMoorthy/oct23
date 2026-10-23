# StubServer v2: Placeholder Syntax

Values in `<angle brackets>` are what the user fills in.

---

**What:** Magic string, JSON request  
**Current:** `${#MockResponse#Request#$['body']['id']}`  
**Syntax type:** `${#MockResponse#Request#<jsonPath>}`  
**Proposed:** `${stub.request.<field.path>}`

---

**What:** Magic string, XML request  
**Current:** `${#MockResponse#Request#//Order[1]/Id[1]}`  
**Syntax type:** `${#MockResponse#Request#<xPath>}`  
**Proposed:** `${stub.request.<xPath>}`

---

**What:** Request body field (SQL/Excel)  
**Current:** `${request.customer.id}`  
**Syntax type:** `${request.<field.path>}`  
**Proposed:** `${stub.request.<field.path>}`

---

**What:** Path parameter (response)  
**Current:** `${id}`  
**Syntax type:** `${<pathParamName>}`  
**Proposed:** `${stub.path.<pathParamName>}`

---

**What:** Path parameter (SQL/Excel)  
**Current:** `${path.id}`  
**Syntax type:** `${path.<pathParamName>}`  
**Proposed:** `${stub.path.<pathParamName>}`

---

**What:** Query parameter (SQL/Excel)  
**Current:** `${query.region}`  
**Syntax type:** `${query.<queryParamName>}`  
**Proposed:** `${stub.query.<queryParamName>}`

---

**What:** Request header  
**Current:** not supported  
**Syntax type:** —  
**Proposed:** `${stub.header.<headerName>}`

---

**What:** Filter value (response)  
**Current:** `${custId}`  
**Syntax type:** `${<filterName>}`  
**Proposed:** `${stub.filter.<filterName>}`

---

**What:** Filter value (SQL/Excel)  
**Current:** `${filter.custId}` / `${filters.custId}`  
**Syntax type:** `${filter.<filterName>}` / `${filters.<filterName>}`  
**Proposed:** `${stub.filter.<filterName>}`

---

**What:** Local / VS property  
**Current:** `${env}`  
**Syntax type:** `${<propertyKey>}`  
**Proposed:** `${stub.prop.<propertyKey>}`

---

**What:** Data generator  
**Current:** `${SequentialNumber}`  
**Syntax type:** `${<generatorVariable>}`  
**Proposed:** `${stub.gen.<generatorVariable>}`

---

**What:** Data source column  
**Current:** `${DB_Two.USER_NAME}`  
**Syntax type:** `${<dataSourceName>.<columnName>}`  
**Proposed:** `${stub.data.<dataSourceName>.<columnName>}`

---

**What:** Data source row count  
**Current:** `${DB_Two.rowCount}`  
**Syntax type:** `${<dataSourceName>.rowCount}`  
**Proposed:** `${stub.data.<dataSourceName>.rowCount}`

---

**What:** Mock service property  
**Current:** `${#MockService#msisdn}`  
**Syntax type:** `${#MockService#<propertyName>}`  
**Proposed:** `${stub.service.<propertyName>}`

---

**What:** Mock service property, `#$` form  
**Current:** `${#MockService#msisdn#$}`  
**Syntax type:** `${#MockService#<propertyName>#$}`  
**Proposed:** `${stub.service.<propertyName>}`

---

**What:** Global property  
**Current:** `${#Global#token}` (blanked today)  
**Syntax type:** `${#Global#<propertyName>}`  
**Proposed:** `${stub.global.<propertyName>}`

---

**What:** Script variable (`context.x = …`)  
**Current:** `${orderId}`  
**Syntax type:** `${<variableName>}`  
**Proposed:** `${stub.var.<variableName>}`

---

**What:** Current date/time  
**Current:** `${=new java.text.SimpleDateFormat("yyyy-MM-dd").format(new java.util.Date())}`  
**Syntax type:** `${=new java.text.SimpleDateFormat("<format>").format(new java.util.Date())}`  
**Proposed:** `${stub.now:<format>}`

---
