---
title: JD Edwards BI Publisher (RTF) Cheat Sheet
description: Practical XPath/XDOXSLT snippets and patterns for JDE BI Publisher RTF templates, formatted for MkDocs Material.
---

# JD Edwards BI Publisher (RTF) Cheat Sheet

A concise reference for working with JD Edwards BI Publisher RTF templates using **XPath 1.0** + **Oracle XSLT Extensions (XDOXSLT/XDOFX)**.

!!! tip "How to read this page"
    - **Copy–paste** the snippets directly into your RTF template (Word + BI Publisher Desktop).  
    - Items marked 🛡️ are *safe patterns* that prevent common runtime errors.  
    - Use the **Variables** pattern to pass values from body to **page headers/footers**.

---

## Core Tags

```xml
<?field?>                                 <!-- print a node value -->
<?for-each:GROUP?> ... <?end for-each?>   <!-- loop -->
<?for-each@section:GROUP?> ... <?end for-each@section?> <!-- loop + page section -->
<?if: condition?> ... <?end if?>          <!-- conditional -->
<?choose:?> <?when: test?> ... <?otherwise:?> ... <?end choose?> <!-- branching -->
<?sort: FIELD1; order=ascending?>         <!-- sort inside loop -->
<?break?>                                 <!-- exit current for-each -->
```

??? info "When to use `@section`"
    Use **`@section`** whenever you need page numbering, headers/footers, or totals **per customer / per document**.  
    It creates a new pagination scope and lets you combine with `<?initial-page-number:1?>`.

```xml
<?for-each@section:A_R_Statement_Detail_S9_Group/On_AR_Branch_S224?>
<?initial-page-number:1?>
... body ...
<?end for-each@section?>
```

---

## Page Numbering

```xml
Page <?page-number?> of <?total-pages?>
```
Place this in the Word header/footer. With `@section` + `<?initial-page-number:1?>`, numbering resets per section.

---

## Variables (Pass Data to Header/Footer)

```xml
<?xdoxslt:set_variable($_XDOCTX,'cust_name', Remit_to_Name_ID445)?>
<?xdoxslt:get_variable($_XDOCTX,'cust_name')?>
```
- **Set** variables inside the XML context (body).  
- **Get** them anywhere (headers/footers have no context).

!!! example "Per-customer wrapper with variables"
    ```xml
    <?for-each@section:CustomerGroup?>
    <?initial-page-number:1?>
    <?xdoxslt:set_variable($_XDOCTX,'cust_name', Name)?>
    ... body ...
    <?end for-each@section?>
    ```
    **Footer**  
    ```xml
    Customer: <?xdoxslt:get_variable($_XDOCTX,'cust_name')?>  
    Page <?page-number?> of <?total-pages?>
    ```

---

## Numbers — Safe Patterns

Avoid **“Cannot convert  to number.”** by guarding conversions.

**Safely format possibly blank or comma’d numbers:**
```xml
<?format-number(
  number(concat('0', normalize-space(translate(string(Total), ',', '')))),
  '#,##0.00'
)?>
```

**Safe comparison to zero:**
```xml
<?if: number(concat('0', normalize-space(translate(string(Amount), ',', '')))) != 0 ?>
  ...
<?end if?>
```

**Shortcut when node may be missing (tolerant):**
```xml
<?format-number(sum(Amount), '#,##0.00')?>   <!-- sum() of empty = 0 -->
```

---

## Strings

```xml
<?xdoxslt:upper_case(Name)?>
<?xdoxslt:lower_case(Name)?>
<?xdoxslt:substring(Text, 1, 10)?>          <!-- 1-based -->
<?xdoxslt:substring_before(Text, '-')?>
<?xdoxslt:substring_after(Text, '-')?>
<?string-length(normalize-space(Text))?>
<?contains(Text, 'ABC')?>                    <!-- often available -->
```

---

## Dates

```xml
<?xdoxslt:format_date(DateNode, 'YYYY-MM-DD')?>
<?xdoxslt:format_date(sysdate(), 'DD Mon YYYY')?>   <!-- current date -->
```

---

## Oracle XDOFX Helpers

```xml
<?xdofx:nvl(Field, 'N/A')?>                       <!-- default when null -->
<?xdofx:decode(Status, 'C','Closed','O','Open','?')?> <!-- switch/case -->
<?xdofx:sysdate()?>                                <!-- now (server time) -->
```

---

## Totals, Counts, Grouping

```xml
<?sum(Invoice/Amount)?>      <!-- inside a group -->
<?count(Invoice)?>
<?min(Amount)?> <?max(Amount)?> <?avg(Amount)?>
```

---

## Sorting in Loops

```xml
<?for-each:Invoice?>
  <?sort: Invoice_Date; data-type=date; order=ascending?>
  <?Invoice_Number?>  <?Invoice_Date?>  <?Amount?>
<?end for-each?>
```

---

## Conditional Blocks

```xml
<?if: normalize-space(TERMS_ID358) != ''?>
  Terms: <?TERMS_ID358?>
<?end if?>
```

---

## Match Data Between Nodes (PageFooters ↔ Customer)

```xml
<?xdoxslt:set_variable($_XDOCTX,'cust_key', Account___local_ID670)?>
<?for-each:/R5603B5001/PageFooters/Page_Footer_S218?>
  <?if: normalize-space(New_Customer_Field) = xdoxslt:get_variable($_XDOCTX,'cust_key')?>
    <?xdoxslt:set_variable($_XDOCTX,'cust_total', Total_Balance_Due___Summary_ID30)?>
    <?break?>
  <?end if?>
<?end-for-each?>
```
Use it later:
```xml
Total Due: <?xdoxslt:get_variable($_XDOCTX,'cust_total')?>
```

---

## Choose / When / Otherwise

```xml
<?choose:?>
  <?when: Status = 'C'?>Closed<?end when?>
  <?when: Status = 'O'?>Open<?end when?>
  <?otherwise?>Unknown<?end otherwise?>
<?end choose?>
```

---

## Repeat Headers in Tables

```xml
<?repeat-header:yes?>
```
Add to the first header row of a Word table to repeat it on each page.

---

## Common Pitfalls & Fixes

| Problem | Fix |
|----------|-----|
| “Cannot convert  to number.” | Use *safe-number* or `sum()` |
| Footer shows wrong customer | Use `set_variable` in body, `get_variable` in footer |
| Wrong equality operator | Use `=` not `==` |
| Whitespace mismatch | Wrap with `normalize-space()` |
| Per-document page totals | Use `@section` + `<?initial-page-number:1?>` |

---

## Quick Copy Blocks

**Per-Customer Wrapper**
```xml
<?for-each@section:CustomerGroup?>
<?initial-page-number:1?>
<?xdoxslt:set_variable($_XDOCTX,'cust_name', Name)?>
... body ...
<?end for-each@section?>
```

**Footer**
```xml
Customer: <?xdoxslt:get_variable($_XDOCTX,'cust_name')?>  
Page <?page-number?> of <?total-pages?>
```

**Show Value Only If Present**
```xml
<?if: string-length(normalize-space(Field)) > 0?>Label: <?Field?><?end if?>
```

---

> 🧠 **Remember:** All logic is **XPath 1.0** with **Oracle XSLT extensions** under the hood.  
> You write XPath in `<? ... ?>` tags; BI Publisher compiles it to XSLT / XSL‑FO when generating output.
