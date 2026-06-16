## devops update 2026-06-16

### gatherwellch.org
- RCA created for downtime during cutover from wix dns to squarespace dns / migration to cloudflare
- Always use HTTPS enabled
- www redirect setup
- domain transfer completed
- monitoring? alerting? on-call?

### ingbert - ltsn
- he experimented with both CryptPad and NextCloud on his own (they both have a limited way to experiment with them for free); they both suck
- participation cannot be a barrier
- i was honest about the fact that we have not spent any meaningful time using either, but we are hoping to start using next cloud soon
- they might decide to be super ambitious and press ahead with us anyway...

### impromptu meeting on 2026-06-13
- project intake infrastructure decision tree minted
- - Sal & Dave started a doc that is meant to serve as a decision tree for the infrastructure needed to support a solution for a customer

### gavin - fcclt - decidim
- intake meeting held
- Determine deployment strategy (Ionos vs. Fly.io) and define maintenance/support parameters.

### fctc-nextcloud
- glanced at coop-cloud's recipe; still need to find somewhere to run their recipe
- started looking into running nextcloud on cloudflare via pulumi/python
- running containers on cloudflare isn't free: https://developers.cloudflare.com/containers/pricing/
- - Workers Paid plan means $5/month minimum

### priority items
- fctc-gitea
- fctc-nextcloud
