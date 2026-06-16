## devops update 2026-06-16

### gatherwellch.org
- RCA created for downtime during cutover from wix dns to squarespace dns / migration to cloudflare
- Always use HTTPS enabled
- www redirect setup
- domain transfer completed
- monitoring? alerting? on-call?

### Ingbert - LTSN
- He experimented with both CryptPad and NextCloud on his own (they both have a limited way to experiment with them for free); they both suck
- - Participation cannot be a barrier
- I was honest about the fact that we have not spent any meaningful time using either, but we are hoping to start using next cloud soon
- They might decide to be super ambitious and press ahead with us anyway...

### Impromptu meeting on 2026-06-13
- Project intake infrastructure decision tree minted
- - Sal & Dave started a doc that is meant to serve as a decision tree for the infrastructure needed to support a solution for a customer

### Gavin - FCCLT - Decidim
- Intake meeting held
- Determine deployment strategy (Ionos vs. Fly.io) and define maintenance/support parameters.

### fctc-nextcloud
- Glanced at coop-cloud's recipe; still need to find somewhere to run their recipe, they say a server
- Started looking into running nextcloud on cloudflare via pulumi/python
- Running containers on cloudflare isn't free: https://developers.cloudflare.com/containers/pricing/
- - Workers Paid plan means $5/month minimum

### Priority items
- fctc-gitea
- fctc-nextcloud
