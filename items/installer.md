### Installer improvements

Our installers should be bulletproof - working on every platform and in most every situation. Additionally 
it'd be good to reduce their complexity as much as possible, moving logic to the app. Then the
app will be a binary that configures its environment when it runs. 

#### Requirements

 * Installation on every platform should be fool-proof
 * Provide great documentation on the installation process
 * Ditch custom installers if at all possible (e.g. by disseminating fallback
   proxy information through KScope or by hand)?
 * Support push of upgrade notifications so we can easily get people off broken
   version of Lantern when necessary

#### Related

[Non-admin installer](non-admin-install.md) will also help the installation process.
