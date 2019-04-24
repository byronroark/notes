# Internal fields:
- `_time`
- `_raw`

# Indexed extractions for json
props.conf << INDEXED_EXTRACTIONS=JSON 

# Great links to check out
demystifying the splunk Common Information Model (CIM)
tutorial data - zip files for testing locally

# Interesting fields data types:
- `a` alpha-numeric
- `#` numeric

# Smart naming conventions
- `group` the name of the group or dept using the knowledge object such as sales, IT etc.
- `object` report, dashboard, macro, etc.
- `description` WeeklySales, FailedLogins, etc.

# `table` command
- `table` command and then comma separate field names as columns
- `dedup` removes duplicates from your results, provide list 
of fields to cleanup dups
    - using `dedup` on 2 fields returns unique combinations that appear once
- `sort` command has `limit` option
    - `+` ascending (default)
    - `-` descending
    - example without space ```... | sort ip, -url```
    - example with space ```... | sort - Vendor, VendorCity```

# `stats` command
> `stats` enables you to calculate statistics on data matching your search
- has built in `as` clause
- `count` returns number of events
- `distinct_count()`, `dc()` - has built in `dedup`, returns count of unique values
- `sum()` returns sum of numeric values
- `avg()` returns an average of number values
- `list` lists all values
- `values()` lists unique values
- `stats count(field)` pass in a field as an argument
    - ex: ```stats count(action) as ActionEvents, count as TotalEvents```
- `distinct_count()` or `dc()` provides a unique count for a given a given field 
    - Ex: ```stats dc(field) as "Field Name"```
    - split using a `by` clause
    - Ex: ```stats dc(field) as "Field Name" by username

# Other commands
- `table` returns table containing only specified fields
- `rename` renames a field in results
- `fields` includes or excludes specified fields
- `dedup` removes duplicates from results
- `sort` sorts results by specified field
- `lookup` adds field values from external source (csv files)

# Transforming commands
- `top`
- `rare`
- `chart`
- `timechart`
- `stats`
- `geostats`

# Return all indexes
```
index=* 
| stats values(sourcetype) as sourcetype by index
```
```
| tstats values(sourcetype) as sourcetype by index
```
# Reporting search commands
- `chart` chart _over_ field _by_ field
- `timechart`
    - `over` clause can be used like `by` clause
    - using `by` clause just use comma to split
    - has default time buckets, can use custom time buckets using `span` option
- only different between `chart` and `stats` command is the ability to multi-series

# Transforming command summary
- `top` / `rare` used for counting frequency of fields
- `stats` can have many fields
- `chart` can have 2 fields
- `timechart` can have 1 field
- `useother=false` filter other series
- `usenull=false` filter null values
- `span` set time value on x-axis

# Create `trendline`
```trendline <trendtype><period>(field) [AS new field]```
- trendtype:
    - `sma` simple moving average
    - `sma2` sma over 2 periods, can be any number
    - `ema` exponential moving average
    - `wma` weighted moving average

# Create `maps` commands
- `iplocation`
- `geostats`
- 'geom`

# Single value visualizations
- `timechart` command
    - `sparkline` inline chart - designed to display time-based trends associated with the primary key
    - `trend` shows direction in which values are moving

# addtotals command
- `fieldname`
- `col` true/false
- `label`
- `labelfield=fieldname`

# Add Total row with column counts
> `row=f` suppresses the default column with row totals 
```
addtotals row=f col=t label=Total labelfield=field_to_total
```

# Eval command
```
index=network sourcetype=cisco_wsa_squid
| stats sum(sc_bytes) as Bytes by usage
| eval megabytes = round(Bytes/(1024*1024),2)
| fields - Bytes
| sort -megabytes
```
OR
```
index=network sourcetype=cisco_wsa_squid
| stats sum(sc_bytes) as Bytes by usage
| eval megabytes = Bytes/(1024*1024), megabytes=round(megabytes,2)
| fields - Bytes
| sort -megabytes
```

# Knowledge object naming conventions
- Group: corresponds to the working groups of the user saving the object (examples: SEG, NEG, OPS, NOC)
- Object Type: Indicates the type of object (alert, report, summary-index-populating) (examples: Alert, Report, Summary)
- Description: A meaningful description of the context and intent of the search, limited to one or two words if possible; ensures the search name is unique 
example: `SEG_Alert_WinEventlogFailures`
- Common Information Model (CIM)

# Field Aliases

# Tags

# Event types

# Macros

# Workflow actions

# Data models
- dataset types:
    - events
    - searches
    - transactions
- event datasets contain constraints and fields
- constraints are essentially the search broken down into a hieryarchy
- fields are properties associated with the events

# Pivot
