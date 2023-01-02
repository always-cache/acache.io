*Incident report*
*2.1.2023*

# Server crash when updating malformed URI

During the new years weekend, a malformed URI in the cache caused two days of downtime for one website in one region.

## Cause

When content is about to expire on acache.io, the underlying caching software (always-cache) updates that content if needed. In order to do this a new HTTP request is constructod based on the initial request and sent to the origin server. In this case, the initial request was for a URI (in practice a URL) that was technically malformed. Thus, the caching software could not construct the request needed for the update. Because of lacking error handling (and the assuption that URIs are always valid), this caused the caching server for that particular site to crash. After a crash, the server is automatically restarted; however, the malformed request was still in the cache and was still due for an update, which caused an infinite loop of crashes.

## Impact

One customer site was impacted, in the data center that received the malformed request. The issue began around noon on new year's eve and was fixed at 11:00 on 2.1.2023 (CET). During this time the entire site was down in the data center region.

## Resolution

The malformed cache entry was removed from the cache. To guard against future similar issues, the underlying bug was fixed and the caching software updated across the entire network.

## Next steps

This incident should have been caught and fixed immediately when it happened. However, because status monitoring is currently manual by nature, and was overlooked during the new year, it was not. In order to ensure our customers' sites are always online no matter what, this will be addressed with the highest priority. In practice this means putting in uptime and status monitoring (with proper alerting), along with automatic failovers in case of issues.

## Timeline

- *2022-12-31 12:36* First failure to reach the affected site in the affected region
- *2023-01-02 10:37* Issue reported by affected customer
- *2023-01-02 11:00* Issue fixed and all systems operational
- *2023-01-02 15:36* Underlying bug fixed and fix deployed in all data centers

All times are in CET.
