# Verify Your Domain

We strongly recommend that you verify the domain or subdomain you will use for sending emails. To do so you have to go to your <a href="https://app.mailjet.com/account/sender/domain" target="_blank">account settings</a> and add the domain you want to use for sending emails. Once the domain is successfully set up we generate an SPF and a DKIM record for you.

<a href="http://www.openspf.org/Introduction" target="_blank">SPF</a> is a DNS TXT record and acts as a filter. It is used to specify a whitelist of IP addresses. This whitelist can be queried by mail clients to see whether a sender IP is in the list.

<a href="http://www.dkim.org/#introduction" target="_blank">DKIM</a> is another DNS TXT record and contains a public key. We automatically generate a public/private key pair for you after registration. The private key is used to sign every message you send. Email clients can then check your DKIM record and use it to verify the signature. Together, SPF and DKIM form the technical basis of email deliverability.

Tip: You can find both records together with instructions in your account settings. Go to your <a href="https://app.mailjet.com/account/setup" target="_blank">account setup page</a> and click on the info button for your domain.

Once your domain is verified you should add the SPF and DKIM records to your domain using the domain configuration tool of your DNS Provider.


You can refer to our step-by-step user guides on creating DNS records:

 - <a href="https://www.mailjet.com/docs/ovh-setup-spf-dkim-record" target="_blank">OVH</a>
 - <a href="https://www.mailjet.com/docs/godaddy-setup-spf-dkim-record" target="_blank">GoDaddy</a>
 - <a href="https://www.mailjet.com/docs/dreamhost-setup-spf-dkim-record" target="_blank">DreamHost</a>
 - <a href="https://www.mailjet.com/docs/1and1-setup-spf-dkim-record" target="_blank">1&1</a>


You can also visit the offical and third-party documentations for DNS providers :

 - Amazon Route 53: <a href="http://anl4u.com/blog/how-to-add-txtspf-dkim-records-for-sub-domain-in-route-53-on-amazon" target="_blank">SPF and DKIM</a>
 - Bluehost: <a href="https://my.bluehost.com/cgi/help/559" target="_blank">General DNS Setup</a>
 - CloudFlare: <a href="https://support.cloudflare.com/hc/en-us" target="_blank">General DNS help</a>
 - Dreamhost: <a href="http://wiki.dreamhost.com/SPF" target="_blank">SPF</a>, <a href="http://wiki.dreamhost.com/DomainKeys_Identified_Mail" target="_blank">DKIM</a>
 - DynDNS: <a href="http://dyn.com/support/record-types-standard-dns/" target="_blank">General DNS setup</a>
 - GoDaddy: <a href="http://support.godaddy.com/help/article/680/managing-dns-for-your-domain-names" target="_blank">SPF and DKIM</a>
 - HostGator: <a href="https://support.hostgator.com/articles/cpanel/how-to-change-dns-zones-mx-cname-and-a-records" target="_blank">General DNS setup</a>
 - Hover: General <a href="https://help.hover.com/entries/21204757-how-to-edit-dns-records-a-cname-mx-txt-and-srv" target="_blank">DNS setup</a>
 - Namecheap: <a href="https://www.namecheap.com/support/knowledgebase/article.aspx/9214/31/email-authentication-tool-in-cpanel-spf-records" target="_blank">SPF</a>, <a href="https://community.namecheap.com/forums/viewtopic.php?f=6&t=5777" target="_blank">DKIM</a>
 - Network Solutions: <a href="http://www.networksolutions.com/support/how-to-manage-advanced-dns-records/" target="_blank">General DNS setup</a>
 - Rackspace: <a href="http://www.rackspace.com/apps/support/portal/1172" target="_blank">General DNS setup</a>
 - Rackspace Cloud DNS: <a href="http://www.rackspace.com/knowledge_center/article/rackspace-cloud-dns" target="_blank">General DNS setup</a>
 - Register.com: <a href="http://help.register.com/app/answers/detail/a_id/487/kw/txt%20record/related/1" target="_blank">General DNS setup</a>
 - United Domains: <a href="http://www.steffen-ille.de/blog.php?scrx=1280&scry=800&entry=15" target="_blank">DKIM and SPF (in German)</a>
 - ZoneEdit: <a href="https://dotster.secure.force.com/zoneeditpkb/KnowledgeArticleDetail?Id=kA260000000CbYd&articleType=How_To__kav" target="_blank">General DNS setup</a>

With some DNS providers the setup can be quite tedious, but we would be happy to help you out. Just contact <a href="https://www.mailjet.com/support/ticket" target="_blank">our support</a>!

<aside class="notice">The validation of a domain can also be initiated with API calls. Prese refer to the <a href="#domains-and-dns">Domains and DNS</a> section for more information.</aside>
