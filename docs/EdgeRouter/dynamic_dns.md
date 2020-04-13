https://community.ui.com/questions/SOLVED-Cloudflare-DDNS-1-9-0/fbe64c70-db03-4449-a046-3edc30aa9e8b

set service dns dynamic interface eth0 service custom-cloudflare protocol cloudflare
set service dns dynamic interface eth0 service custom-cloudflare server api.cloudflare.com/client/v4/
set service dns dynamic interface eth0 service custom-cloudflare host-name dyn.example.com
set service dns dynamic interface eth0 service custom-cloudflare login "cloudflare@example.com"
set service dns dynamic interface eth0 service custom-cloudflare password "93c86aa10fbd56bfea6820d28f25b3b955573557"
set service dns dynamic interface eth0 service custom-cloudflare options "zone=example.com use=web ssl=yes"

----------------------------------------------------------------------------------

go to config tree > service > dns > dynamic (hit the +, this enables ddns)

> interface eth0 (wan interface - mine is eth0)

> eth0 (leave web/web-skip blank)

> service custom-cloudflare (needs to be prefixed with 'custom-', otherwise will error out)

> custom-cloudflare

host-name sub.mydomain.com

login mycloudflareemail@email.com

options zone=mydomain.com

password <myGlobalCloudFlareAPITokenGoesHere> (on your cloudflare profile, regenerate your global API token before starting this [myprofile > API Tokens > Global API Key > Change] - for some reason it may not like the first API token that is generated for you)

protocol cloudflare

server api.cloudflare.com/client/v4/ (this is the direct link to their RESTful API, use this. not their webpage. [www.cloudflare.com])

----------------------------------------------------------------------------------

on Cloudflare DNS Records:

Create an A record

Type Name Content TTL ProxyStatus

A mysubdomain 127.0.0.1 2min DNS Only

I noticed this in cloudflare's DDNS area on their RESTful API page. This was the last step I made for the ddns to successfully sync and point to my WAN IP.

----------------------------------------------------------------------------------

Once this is all set you can ssh/use the CLI tool within the web interface and run:

update dns dynamic interface eth0

show dns dynamic status

interface  : eth0

[ Status will be updated within 60 seconds ]

show dns dynamic status

interface  : eth0

ip address  : XX.XX.XX.XX

host-name  : sub.mydomain.com

last update : Tue Mar 31 00:08:46 2020

update-status: good