- Trong thực tế việc các component call API có thể giống nhau vì vậy để tránh lặp lại code thì ta có thể tạo ra một custom hook để tái sử dụng lại code.

```js title="SuperHeroHook.js"
import { useQuery } from "react-query";
const getDataSuperHeroes = () => {
  console.log("ok");
  return axios.get("http://localhost:4000/SuperHeroes");
};
export const SuperHeroesHooks = (onSuccess, onError) => {
  return useQuery("super-heroes", getDataSuperHeroes, {
    onSuccess: onSuccess,
    onError: onError,
  });
};
```

```js title="SuperHero.page.js"
import { SuperHeroesHooks } from "./SuperHeroHook";
const SuperHeroes = () => {
  const onSuccess = (data) => {
    console.log("data", data);
  };
  const onError = (error) => {
    console.log("error", error);
  };
  const { data, isLoading, error } = SuperHeroesHooks(onSuccess, onError);
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error...</div>;
  return (
    <div>
      {data?.data.map((superHero) => (
        <div key={superHero.id}>{superHero.name}</div>
      ))}
    </div>
  );
};
```
