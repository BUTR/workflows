## Steam Workflow
If Steam Guard is used, you need the `STEAM_AUTH_CODE` secret. Follow this url as a guide:  
https://github.com/SteamTimeIdler/stidler/wiki/Getting-your-%27shared_secret%27-code-for-use-with-Auto-Restarter-on-Mobile-Authentication#getting-shared-secret-from-ios-windows  
You need to either switch to Steam Desktop Authenticator or extract the Shared Key secret from your phone's files.  
If Steam Guard is not used, `STEAM_LOGIN` and `STEAM_PASSWORD` will be enough.

## NexusMods Workflow
Uploads are done via the official NexusMods v3 API and only require an API key. Get one at https://www.nexusmods.com/settings/api-keys - this is our `NEXUSMODS_APIKEY` (`NEXUSMODS_API_KEY` for the verify workflow) secret.

Session cookies (`NEXUSMODS_SESSION_COOKIE`) are no longer used - the cookie-based `refresh-unex-nexusmods.yml` and `verify-nexusmods.yml` workflows were removed.
