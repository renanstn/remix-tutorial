# Welcome to Remix!

- [Remix Docs](https://remix.run/docs)

## Development

From your terminal:

```sh
npm run dev
```

This starts your app in development mode, rebuilding assets on file changes.

## Deployment

First, build your app for production:

```sh
npm run build
```

Then run the app in production mode:

```sh
npm start
```

Now you'll need to pick a host to deploy it to.

### DIY

If you're familiar with deploying node applications, the built-in Remix app server is production-ready.

Make sure to deploy the output of `remix build`

- `build/server`
- `build/client`

## Tutorial notes

- Link do tutorial: https://remix.run/docs/en/main/start/tutorial

- Um link CSS foi exportado em `root.tsx` utilizando:

```ts
export const links: LinksFunction = () => [
  { rel: "stylesheet", href: appStylesHref },
];
```

> Every route can export a links function. They will be collected and rendered into the <Links /> component we rendered in app/root.tsx.

- O nome dos arquivos na pasta `routes` se tornam automaticamente os nomes das rotas
    - O caractere especial `.` vira `/`
    - O caractere especial `$` vira um ID

> In the Remix route file convention, `.` will create a `/` in the URL and `$` makes a segment dynamic.

[Mais informações sobre file naming](https://remix.run/docs/en/main/file-conventions/routes)

- O componente `Outlet` é usado para renderizar "coisas dentro de coisas".

- Devemos utilizar o componente `Link` para fazer links ao invés do clássico `<a href>`, pois o `<a href>` faz um reload completo na página, enquanto o componente Link fará a mágica de carregar somente o componente interno:

> Client side routing allows our app to update the URL without requesting another document from the server. Instead, the app can immediately render new UI. Let's make it happen with <Link>.
```ts
<Link to={`/contacts/1`}>Your Name</Link>
```

- Após fazer a mudança acima, o browser **não fará** novos requests ao clicar nos links.

- Funções `loader` são sempre utilizadas para carregar dados ao acessar a página:

```ts
import { useLoaderData } from "@remix-run/react";
export const loader = async () => {
  const contacts = await getContacts();
  return json({ contacts });
};

export default function App() {
  const { contacts } = useLoaderData<typeof loader>();
// ...
```

- Utilizamos `NavLink` para destacar o item ativo na sidebar

- O `useNavigation` é utilizado para saber se a página está carregando ainda ou não, e assim animar elementos para melhor feedback

```ts
import { useNavigation } from "@remix-run/react";

export default function App() {
  const navigation = useNavigation();

<div className={navigation.state === "loading" ? "loading" : ""} id="detail">
    {/* Page content goes here!!! */}
    <Outlet />
</div>
```

- O `useFetcher` serve para ativar o `action` ou o `loader` **sem causar uma navegação**.
    - A função de **favoritar usuário** contém um exemplo disso.
