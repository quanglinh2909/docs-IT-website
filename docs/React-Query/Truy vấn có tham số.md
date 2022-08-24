- Có ba case để truy vấn dữ liệu với tham số:

## Cách 1:

```js title="SuperHeroHook.js"
import axios from "axios";
import { useQuery } from "react-query";
export const useSuperHeroData = (heroId) => {
  return useQuery(["super-hero"], () =>
    axios.get("http://localhost:4000/SuperHeroes/" + heroId)
  );
};
```

:::danger Lưu ý

- không nên dùng cách này

:::

## Cách 2: Thường dùng

```js title="SuperHeroHook.js"
import axios from "axios";
import { useQuery } from "react-query";
const getDataSuperHeroes = (heroId) => {
  return axios.get("http://localhost:4000/SuperHeroes/" + heroId);
};
export const useSuperHeroData = (heroId) => {
  return useQuery(["super-hero", heroId], () => getDataSuperHeroes(heroId));
};
```

## Cách 3:

```js title="SuperHeroHook.js"
import axios from "axios";
import { useQuery } from "react-query";
const getDataSuperHeroes = ({ queryKey }) => {
  const heroId = queryKey[1];
  return axios.get("http://localhost:4000/SuperHeroes/" + heroId);
};
export const useSuperHeroData = (heroId) => {
  return useQuery(["super-hero", heroId], getDataSuperHeroes);
};
```

:::tip Chú ý

Khi sử dụng cách 2 và 3 thì khi heroId thay đổi thì nó sẽ tự động refetch data

:::
