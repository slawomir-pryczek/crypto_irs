# Crypto_irs
Process millions of trades to calculate taxes (IRS)

# Prerequisites
There are certain presumptions about input files. First of all, it should be CSV in following format:

- time - Unix timestamp in UTC
- buy - Buy amount, dot is decimal separator. After the buy column there should be additional column with any name, which will be treated as buy currency.
- sell - Sell amount, same principe as buy apply.
- fee - Fee amount, same principe as buy apply.
- type - Event type, which will be mapped to internal type. See below.
- trade_group - Trade group (informational)
- tradeid - Trade id (informational)
- added - Informational
- comment - Comment field, include "otc" to trigger OTC queue
- buy_value_fiat - Fiat value of assets gained
- sell_value_fiat - Fiat value of assets lost

Example header follows:
<code>time,buy,cur.,sell,cur.,fee,cur.,type,ct,buy_value_btc,buy_value_fiat,sell_value_btc,sell_value_fiat,ex1,ex2,trade_group,tradeid,added,comment</code>

# Event types and mapping
The processor will map events to several internal event types.
- trade -> exchange of coin A to coin B
- deposit -> deposit to account or exchange
- withdraw -> withdraw from account or exchange
- in -> gain, this means that you gained some tokens from airdrop, mining, etc.
- out -> loss, you lost some amounts of tokens due to fees, spending

<pre>
trade => trade

deposit => deposit
withdraw => withdraw

income, mining, gift/tip,donation, gift => in
out, lost, stolen => out
</pre>

# Fetching input
Input will be fetched from **Workdir** which is defined in cfg.go and defaults to ./in/ subdirectory. All csv and txt files will be read and data will be sorted. If the file will contain "\_off\_" inside its name it won't be included in calculations. Text files (txt) will be treated as csv. Each file can have different data organizaion which will be determined from the header.

After loading the dataset it'll be sorted if needed. It's recommended to split files into small batches, so they can be easily edited.


