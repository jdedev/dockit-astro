---
title: BIP Knowledge Base
---

# BIP Knowledge Base

## Define Numeric Format

```xml
<xsl:decimal-format xdofo:ctx="begin" name="myFMT" grouping-separator="." decimal-separator=","/>
```

### Use Format

```xml
<?format-number(var_F03B11_AG_ID142, '##0,00', 'myFMT')?>
```

### Use Format Conditionally

```xml
<?if:number(Grand_Total_ID8)!=0?><?format-number(Grand_Total_ID8, '#,##0.00', 'myFMT')?><?end if?>
```
## Summarization

```xml
<?format-number(sum(Gross_Amount___Display_ID28)+sum(Amount_Due___Display_ID20), '#,##0.00')?>
```

## Grouping 

```xml
<?for-each:Phase_1___Build_Work_File_S1/Detail_Line_1_Section_S2_Group/On_Payment_Terms_S3/*[self::With_Lot___Main_S39 or self::With_Lot___Lot_S36 or self::Print_Business_Reason_S22]?>
```

## Validation

```xml
<?if:number(CL__Extended_Amount_ID13)!=0?>

<?if:normalize-space(GRAND_TOTAL_ID22)!=''?>

```

## Bursting

```xml
/R5942565/Phase_1___Build_Work_File_S1/Detail_Line_1_Section_S2_Group/On_Payment_Terms_S3 
```