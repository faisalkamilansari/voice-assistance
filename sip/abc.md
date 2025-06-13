# Twilio Setup
twilio login 
SID 
Auth
friendly-name

twilio profiles:use friendly-name

twilio api trunking v1 trunks create --friendly-name "My test trunk" --domain-name "buopso-test.pstn.twilio.com"
SID                                 Friendly Name  Domain Name
TK6e0396c9315c1e53815bf92394b51d1d  My test trunk  buopso-test.pstn.twilio.com

## inbound
twilio api trunking v1 trunks origination-urls create --trunk-sid TK6e0396c9315c1e53815bf92394b51d1d --friendly-name "buopso-test SIP URI" --sip-url "sip:16vjw122p6z.sip.livekit.cloud;transport=tcp" --weight 1 --priority 1 --enabled
SID                                 Friendly Name        Sip URL                                          Priority  Weight
OU8e4452a84fa81edb60f177b55989bad4  buopso-test SIP URI  sip:16vjw122p6z.sip.livekit.cloud;transport=tcp  1         1   

##phone number list
twilio phone-numbers list
SID                                 Phone Number  Friendly Name
PN1577a8f0f84f57929fd99047f81e8b37  +16282286776  (628) 228-6776


twilio api trunking v1 trunks phone-numbers create --trunk-sid TK6e0396c9315c1e53815bf92394b51d1d --phone-number-sid PN1577a8f0f84f57929fd99047f81e8b37
SID                                 Friendly Name   Phone Number
PN1577a8f0f84f57929fd99047f81e8b37  (628) 228-6776  +16282286776

twilio api trunking v1 trunks phone-numbers create --trunk-sid TK6e0396c9315c1e53815bf92394b51d1d --phone-number-sid PN1577a8f0f84f57929fd99047f81e8b37