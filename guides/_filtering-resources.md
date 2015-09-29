#Filtering resources

```bash
https://api.mailjet.com/v3/REST/endpoint?filter1=this&filter2=that&filter3=it
```

The Mailjet API provides a set of general filters that can be applied to a <code>GET</code> request for each resource.  In addition to these general filters, each API resource has its own filters that can be used when performing the <code>GET</code>.  You can find these filters in their respective [resource description](/email-api/v3/).

To use a filter in a <code>GET</code>, you can amend the resource URL with a standard query string (<code>?filter1=this&filter2=that&filter3=it</code>).


##The Limit Filter

{{contactFilteringA_GET}}

You can limit the number of results by applying the <code>Limit</code> filter. The default value is 10 and the maximum value is 1000. 'Limit=0' delivers the maximum amount of results - 1000.


##The Offset Filter

{{contactFilteringB_GET}}
You can use the <code>Offset</code> filter to retrieve a list starting from a certain offset.
<div></div>

{{contactFilteringC_GET}}
The <code>Offset</code> filter can be combined with the <code>Limit</code> filter. 


##The Sort Filter

{{contactFilteringD_GET}}

You can sort a GET query by specifying a property name as the value of the 'Sort' filter. The default sorting is ascending.  When a property name is postfixed with <code>+DESC</code> , the sort order is descending.  

Please note that the <code>Sort</code> filter does not work with all properties and that the <code>+DESC</code> is not available for all properties.

<div></div>
{{contactFilteringE_GET}}

To retrieve the same query only in descending order, amend the URL with <code>+DESC</code>:

##Resource Filters

{{contactFilteringRessource_GET}}

On each resource, the API provide specific filters. Visit the [reference](/email-api/v3/) to see what is available.

## Unique Key Filters

{{contactFilteringUKey_GET}}

You can access each unique element using unique key filter. Visit the [reference](/email-api/v3/) to see the keys available for each resource.

<div></div>
{{eventcallbackurlfilter_GET}}

In some case the unique key consist of several informations, you can call this unique key combinaison by using the seperator <code>|</code>.

##The Count Filter

{{contactFilteringF_GET}}

> API response:

```json
{
	"Count": 152,
	"Data": [ ],
	"Total": 152
}
```

Use the filter <code>countOnly</code> to retrieve the number of records a resource will return. This filter will not extract any list of results but only count them. 

<aside class="notice">When you call a resource without the filter <code>countOnly</code>, <code>Count</code> and <code>Total</code> will only show you the number of elements extracted and not the global number.</aside>

##Navigation through results

To navigate on a full set of results, we advise you to either use the filter <code>countOnly</code> to know how many pages of results you will need to extract or to simply loop with a change of <code>Offset</code> until you reach an empty set of results. 





