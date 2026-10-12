## Steam Workflow
If Steam Guard is used, you need the `STEAM_AUTH_CODE` secret. Follow this url as a guide:  
https://github.com/SteamTimeIdler/stidler/wiki/Getting-your-%27shared_secret%27-code-for-use-with-Auto-Restarter-on-Mobile-Authentication#getting-shared-secret-from-ios-windows  
You need to either switch to Steam Desktop Authenticator or extract the Shared Key secret from your phone's files.  
If Steam Guard is not used, `STEAM_LOGIN` and `STEAM_PASSWORD` will be enough.

## Certum Signing Workflow
`sign-certum.yml` asks [Aragas/code-signing](https://github.com/Aragas/code-signing) to sign an artifact of the calling run with the Certum Code Signing in the Cloud certificate, waits for it and replaces the artifact with the signed one, unless `signed_artifact_name` is set.  
The Certum secrets live only in code-signing. It signs only what its `config.json` lists for the repository, approved by its approvers, see its README. It needs:
* `CODE_SIGNING_CLIENT_ID` secret - the client ID of the Code Signing Requester GitHub App.
* `CODE_SIGNING_PRIVATE_KEY` secret - that app's private key. It can only ask for signing.

```yaml
  sign:
    needs: ["build"]
    uses: BUTR/workflows/.github/workflows/sign-certum.yml@master
    with:
      artifact_name: bannerlord
    secrets:
      CODE_SIGNING_CLIENT_ID: ${{ secrets.CODE_SIGNING_CLIENT_ID }}
      CODE_SIGNING_PRIVATE_KEY: ${{ secrets.CODE_SIGNING_PRIVATE_KEY }}
```

`release-nuget.yml` signs the binaries inside its packages before pushing them with `sign: true`. The packages travel as an artifact, its name has to be the one code-signing's `config.json` lists for the repository:
```yaml
  publish-on-nuget:
    uses: BUTR/workflows/.github/workflows/release-nuget.yml@master
    with:
      project_path: src/Bannerlord.ButterLib/Bannerlord.ButterLib.csproj
      sign: true
      artifact_name: nuget
    secrets:
      NUGET_API_KEY: ${{ secrets.NUGET_API_KEY }}
      CODE_SIGNING_CLIENT_ID: ${{ secrets.CODE_SIGNING_CLIENT_ID }}
      CODE_SIGNING_PRIVATE_KEY: ${{ secrets.CODE_SIGNING_PRIVATE_KEY }}
```

## NexusMods Workflow
Uploads are done via the official NexusMods v3 API and only require an API key. Get one at https://www.nexusmods.com/settings/api-keys - this is our `NEXUSMODS_APIKEY` (`NEXUSMODS_API_KEY` for the verify workflow) secret.

Session cookies (`NEXUSMODS_SESSION_COOKIE`) are no longer used - the cookie-based `refresh-unex-nexusmods.yml` and `verify-nexusmods.yml` workflows were removed.
