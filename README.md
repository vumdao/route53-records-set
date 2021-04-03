<p align="center">
  <a href="https://dev.to/vumdao">
    <img alt="Ensure No Product Tenants Are Opened During Maintenance Mode" src="https://github.com/vumdao/route53-records-set/blob/master/cover.png?raw=true" width="700" />
  </a>
</p>
<h1 align="center">
  <div><b>Ensure No Product Tenants Are Opened During Maintenance Mode</b></div>
</h1>


### 🚀 **How to set maintenance mode for thoudsands of tenants**
- Create a record set call `alb-tunnel.cloudopz.com` as a CNAME point to the ALB
- Set aLL Tenant records point to `alb-tunnel.cloudopz.com` with type CNAME
- So when it needs to set maintainance mode, we just need to set `alb-tunnel.cloudopz.com` point to our maitainance page


### 🚀 **Why need to Ensure No Product Tenants Are Opened During Maintenance Mode**
- During maintainance mode, the database might be running migrated so customer should not access the site
- We need to ensure no tenant product point to ALB directly except test/demo sites. Here is the way to manually check (free feel to automate it)

```
aws route53  list-resource-record-sets --hosted-zone-id Z39FXXXXXXXXXX --query "ResourceRecordSets[?Type == 'CNAME']" | jq -r '.[] | [([.Name, .ResourceRecords[].Value] | join(", "))] | @csv' | 
grep "alb-.*..amazonaws.com" | grep -Ev "test|demo" 
```