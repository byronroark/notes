# Sample SPL statements

```
index=web sourcetype=access_combined action=*
| transaction JSESSIONID
| table JSESSIONID, clientip, action
| search action=purchase
```

```
index=web sourcetype=access_combined action=*
| transaction JSESSIONID
| table JSESSIONID, clientip, duration, eventcount, action
| search action=purchase
```

```
index=web sourcetype=access_combined action=*
| transaction JSESSIONID
| table JSESSIONID, clientip, duration, eventcount, action
| search action=purchase
| eval durationMinutes = round((duration/60),1)
```

```
index=web sourcetype=access_combined action=*
| transaction JSESSIONID
| table JSESSIONID, clientip, duration, eventcount, action
| search action=purchase
| eval durationMinutes = round((duration/60),1)
| search durationMinutes>1
```

```
(index=web sourcetype=access_combined) OR (index=network sourcetype=cisco_wsa_squid) 
| transaction status maxspan=15m
| search sourcetype=access_combined AND sourcetype=cisco_wsa_squid
```

```
index=sales sourcetype=vendor_sales VendorCountry=Germany OR VendorCountry=France OR VendorCountry=Italy
| stats count as "Total", sum(sale_price) as "USD" by product_name
| table product_name, Total, USD
| rename product_name as "Product Name"
```

```
index=sales sourcetype=vendor_sales VendorCountry=Germany OR VendorCountry=France OR VendorCountry=Italy
| stats count as "Total", sum(sale_price) as "USD" by product_name
| table product_name, Total, USD
| rename product_name as "Product Name"
| eval USD = "$" + tostring(USD, "commas")
```