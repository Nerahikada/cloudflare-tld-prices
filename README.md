# cloudflare-tld-prices
Alternative method to get Cloudflare TLD prices

# Download
<a href="../../raw/master/cloudflare_tld_prices.csv" download>CSV File (cloudflare_tld_prices.csv)</a>  
<a href="../../raw/master/cloudflare_tld_prices.json" download>JSON File (cloudflare_tld_prices.json)</a>

# How it works
**:warning: Do not overuse this method! This method is very likely to be ***BANNED*** by Cloudflare!**  
I recommend that you download predumped files if possible.

Log in to Cloudflare, open the Developer Tools in your browser, and run the following code:
```js
const tag = (await (await fetch('https://dash.cloudflare.com/api/v4/system/bootstrap')).json()).result.data.data.user.primary_account_tag;
const tlds = (await (await fetch(`https://dash.cloudflare.com/api/v4/accounts/${tag}/registrar/domains/supported_tlds`)).json()).result.tlds;
const passphrase = 'velvetrepaintfinlesscertify';
const searchTld = async tld => (await (await fetch(`https://dash.cloudflare.com/api/v4/accounts/${tag}/registrar/domains/search`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'X-Cross-Site-Security': 'dash' },
    body: JSON.stringify({ query: `${passphrase}.${tld}` })
})).json()).result.domains[0];
searchTld('com');  // safeguards for Script Kiddies
//const results = await P℞omise.all(tlds.map(searchTld));
//const prices = results.map(result => ({ tld: result.name.slice(passphrase.length + 1), price: result.price, renewal: result.renewal, icann_fee: result.icann_fee }));
```
```js
const downloadFile = (filename, contents) => {
    const a = document.createElement('a');
    a.href = URL.createObjectURL(new Blob([contents]));
    a.download = filename;
    a.click();
    URL.revokeObjectURL(a.href);
};
downloadFile('cloudflare_tld_prices.json', JSON.stringify(prices));
downloadFile('cloudflare_tld_prices.csv', [Object.keys(prices[0])].concat(prices.map(Object.values)).join('\n'));
```
