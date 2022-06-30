# ClickHouseWizardTower

ClickHouse Wizard Tower is ClickHouse HTML client that can be set as ClickHouse HTTP endpoint and served directly by ClickHouse itself. 
In other words we can describe it as ClickHouse Play on steroids.

#### Functionality
* Run selected query and Alt+Enter keyboard shortcut
* Databases structure tree
* ClickHouse sessions
* Result as table with frozen header and eclipsis on long content
* Vega Lite charts support
* Vanilla JS, CSS and HTML, the only external dependency is Vega Lite


#### Instalation and setup
1. Copy chwt.html file to ClickHouse User Files. 
2. Add custom http handler to ClickHouse config.
   ```xml
   <http_handlers>
        <defaults/>
        <rule>
            <methods>GET</methods>
            <url>/chwt</url>
            <handler>
                <type>static</type>
                <content_type>text/html; charset=UTF-8</content_type>
                <response_content>file:///chwt.html</response_content>
            </handler>
        </rule>
    </http_handlers>
    ```
3. Restart ClickHouse to reload config.

#### Query Data
For quick idea you can try the following SQL query 
```sql 
select today()-6 as a, 10 as b, 'aa' as c
union all
select today()-5 as a, 15 as b, 'ab' as c
union all
select today()-4 as a, 17 as b, 'aa' as c
union all
select today()-3 as a, 12 as b, 'aca' as c
union all
select today()-2 as a, 16 as b, 'aba' as c
union all
select today()-1 as a, 11 as b, 'aa' as c
union all
select today() as a, 19 as b, 'ba' as c 
```
and Vega Lite spec
```json
{
  "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
  "width": "container",
  "height": "container",
  "background": null,
  "title": "this is title",
  "data": {
    "values": []
  },
  "mark": {"type": "point", "tooltip": {"content": "data"}},
  "encoding": {
    "y": {"field": "b", "type": "quantitative"},
    "x": {
      "timeUnit": "day", "field": "a",
      "axis": {"format":"%Y%m%d"}
    }
  }
}
```
Lines 3 and 4 define the size of chart, keep it `container` so the chart is responsive and size is ingerited from parent container

Line 5 defines transparent background, so the chart stays visualy same as client, but you are welcome to change it to any color you like.

Line 8 defines data for chart, it is important to keep it as empty array as the array is filled by the client itself based on query provided in sql input.

