---
layout: default
title: "ContainerLayout"
---

# ContainerLayout

This [Enterprise Edition](https://marmelab.com/ra-enterprise)<img class="icon" src="./img/premium.svg" /> component offers an alternative to react-admin's `<Layout>` for applications with a limited number of resources. It replaces the sidebar menu by an AppBar menu, and displays the content in a centered container.

![Container layout](https://marmelab.com/ra-enterprise/modules/assets/ra-navigation/latest/container-layout.png)

## Usage

Set `<ContainerLayout>` as the `<Admin layout>` value:

```jsx
import { Admin, Resource } from 'react-admin';
import { ContainerLayout } from '@react-admin/ra-navigation';

export const App = () => (
    <Admin dataProvider={dataProvider} layout={ContainerLayout}>
        <Resource name="songs" list={SongList} />
        <Resource name="artists" list={ArtistList} />
    </Admin>
);
```

See more details in the [ra-navigation documentation](https://marmelab.com/ra-enterprise/modules/ra-navigation#containerlayout).

## Props

`<ContainerLayout>` accepts the following props:

-   `menu`: The menu component to use. Defaults to `<HorizontalMenu>`.
-   `appBar`: The component to use to render the top AppBar. Defaults to `<Header>`
-   `toolbar`: The buttons to render on the top right of the toolbar.
-   `maxWidth`: The maximum width of the content `<Container>`. Defaults to `md`.
-   `fixed`: Whether the content `<Container>` should be fixed. Defaults to false.

## `menu`

By default, `<ContainerLayout>` renders one menu item per resource in the admin. To reorder the menu, omit resources, or add custom pages, pass a custom menu element to the `menu` prop. This element should be [a `<HorizontalMenu>` component](./HorizontalMenu.md) with `<HorizontalMenu.Item>` children. Each child should have a `value` corresponding to the [application location](https://marmelab.com/ra-enterprise/modules/ra-navigation#concepts) of the target, and can have a `to` prop corresponding to the target location if different from the app location.

```jsx
import {
    Admin,
    Resource,
    CustomRoutes,
    ListGuesser,
    EditGuesser,
} from 'react-admin';
import { Route } from 'react-router-dom';
import {
    ContainerLayout,
    HorizontalMenu,
    useDefineAppLocation,
} from '@react-admin/ra-navigation';

const Menu = () => (
    <HorizontalMenu>
        <HorizontalMenu.Item label="Dashboard" to="/" value="" />
        <HorizontalMenu.Item label="Songs" to="/songs" value="songs" />
        <HorizontalMenu.Item label="Artists" to="/artists" value="artists" />
        <HorizontalMenu.Item label="Custom" to="/custom" value="custom" />
    </HorizontalMenu>
);

const MyLayout = props => <ContainerLayout {...props} menu={<Menu />} />;

const CustomPage = () => {
    useDefineAppLocation('custom');
    return <h1>Custom page</h1>;
};

const Dashboard = () => <h1>Dashboard</h1>;
const CustomPage = () => <h1>Custom page</h1>;

export const App = () => (
    <Admin dataProvider={dataProvider} layout={MyLayout} dashboard={Dashboard}>
        <Resource name="songs" list={ListGuesser} edit={EditGuesser} />
        <Resource name="artists" list={ListGuesser} edit={EditGuesser} />
        <CustomRoutes>
            <Route path="custom" element={<CustomPage />} />
        </CustomRoutes>
    </Admin>
);
```

## `appBar`

If you want to use a different color for the AppBar, or to make it sticky, pass a custom `appBar` element based on `<Header>`, which is a simple wrapper around [MUI's `<AppBar>` component](https://mui.com/material-ui/react-app-bar/#main-content).

```jsx
import { ContainerLayout, Header } from '@react-admin/ra-navigation';

const myAppBar = <Header color="primary" position="sticky" />;
const MyLayout = props => <ContainerLayout {...props} appBar={myAppBar} />;
```

## `toolbar`

The `toolbar` prop allows to add buttons to the top right of the toolbar. It accepts an element.

```jsx
import { LocalesMenuButton, LoadingIndicator } from 'react-admin';
import { ContainerLayout } from '@react-admin/ra-navigation';

const toolbar = (
    <>
        <LocalesMenuButton />
        <LoadingIndicator />
    </>
);
const MyLayout = props => <ContainerLayout {...props} toolbar={toolbar} />;
```

## `maxWidth`

This prop allows to set the maximum width of the content [`<Container>`](https://mui.com/material-ui/react-container/#main-content). It accepts a string, one of `xs`, `sm`, `md`, `lg`, `xl`, or `false` to remove side margins and occupy the full width of the screen.

```jsx
import { ContainerLayout } from '@react-admin/ra-navigation';

const MyLayout = props => <ContainerLayout {...props} maxWidth="md" />;
```

## `fixed`

If you prefer to design for a fixed set of sizes instead of trying to accommodate a fully fluid viewport, you can set the `fixed` prop. The max-width matches the min-width of the current breakpoint.

```jsx
import { ContainerLayout } from '@react-admin/ra-navigation';

const MyLayout = props => <ContainerLayout {...props} fixed />;
```
