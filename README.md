# cloudflare-tld-prices
Alternative method to get Cloudflare's TLD prices

# Download
TODO

# How it works
**:warning: Do not overuse this method! This method is very likely to be ***BANNED*** by Cloudflare!**  
I recommend downloading pre-dumped files if possible.

Login to Cloudflare, open the Developer Tools in your browser and execute the following code:
```js
const tag = (await (await fetch('https://dash.cloudflare.com/api/v4/system/bootstrap')).json()).result.data.data.user.primary_account_tag;
const tlds = (await (await fetch(`https://dash.cloudflare.com/api/v4/accounts/${tag}/registrar/domains/supported_tlds`)).json()).result.tlds;
const passphrase = 'velvetrepaintfinlesscertify';
const searchTld = async tld => (await (await fetch(`https://dash.cloudflare.com/api/v4/accounts/${tag}/registrar/domains/search`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'X-Cross-Site-Security': 'dash' },
    body: JSON.stringify({ query: `${passphrase}.${tld}` })
})).json()).result.domains[0];
const results = await Promise.all(tlds.map(searchTld));
const prices = results.map(result => ({ tld: result.name.slice(passphrase.length + 1), price: result.price, renewal: result.renewal, icann_fee: result.icann_fee }));
```
