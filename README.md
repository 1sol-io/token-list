# @onesol/spl-token-registry

[![GitHub license](https://img.shields.io/badge/license-APACHE-blue.svg)](https://github.com/1sol-io/token-list/blob/main/LICENSE)

Solana Token Registry is a package that allows application to query for list of tokens.
The JSON schema for the tokens includes: chainId, address, name, decimals, symbol, logoURI (optional), tags (optional), and custom extensions metadata.


## TokenList json URL
[https://cdn.jsdelivr.net/gh/1sol-io/token-list@main/src/tokens/solana.tokenlist.json](https://cdn.jsdelivr.net/gh/1sol-io/token-list@main/src/tokens/solana.tokenlist.json)

## TokenSwap Pools json URL
[https://cdn.jsdelivr.net/gh/1sol-io/token-list@main/src/pools/1sol.pools.json](https://cdn.jsdelivr.net/gh/1sol-io/token-list@main/src/pools/1sol.pools.json)

## SerumDex Markets json URL
[https://cdn.jsdelivr.net/gh/1sol-io/token-list@main/src/markets/1sol.markets.json](https://cdn.jsdelivr.net/gh/1sol-io/token-list@main/src/markets/1sol.markets.json)

## Examples

### Query available tokens

```typescript
new TokenListProvider().resolve().then((tokens) => {
  const tokenList = tokens.filterByClusterSlug('mainnet-beta').getList();
  console.log(tokenList);
});
```

### Render icon for token in React

```typescript jsx
import React, { useEffect, useState } from 'react';
import { TokenListProvider, TokenInfo } from '@solana/spl-token-registry';


export const Icon = (props: { mint: string }) => {
  const [tokenMap, setTokenMap] = useState<Map<string, TokenInfo>>(new Map());

  useEffect(() => {
    new TokenListProvider().resolve().then(tokens => {
      const tokenList = tokens.filterByChainId(ENV.MainnetBeta).getList();

      setTokenMap(tokenList.reduce((map, item) => {
        map.set(item.address, item);
        return map;
      },new Map()));
    });
  }, [setTokenMap]);

  const token = tokenMap.get(props.mint);
  if (!token || !token.logoURI) return null;

  return (<img src={token.logoURI} />);

```
