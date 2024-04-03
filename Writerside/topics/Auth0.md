# Auth0

Auth0 is a service that provides authentication and authorization as a service. It is a cloud-based service that provides a set of tools and services that allow developers to add authentication and authorization to their applications. Auth0 provides a set of APIs that allow developers to integrate authentication and authorization into their applications.

## Environment Variables

Auth0 domain and client ID are stored as environment variables in the .env file. Here's an example from your .env.example file:

```Text
VITE_AUTH0_DOMAIN=
VITE_AUTH0_CLIENT_ID=
```

You can find your Auth0 `domain` and `clientID` in the Auth0 dashboard.

## Auth0Provider Setup

The Auth0Provider component from the `@auth0/auth0-react` package is used in your `src/main.tsx` file to wrap your main App component. This sets up the Auth0 context for your app.

Here's an example of how to set up the Auth0Provider in your `src/main.tsx` file:

```Typescript
import { Auth0Provider } from '@auth0/auth0-react'
import App from './App.tsx'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <Auth0Provider
      // access environment variables
      domain={import.meta.env.VITE_AUTH0_DOMAIN}
      clientId={import.meta.env.VITE_AUTH0_CLIENT_ID}
      authorizationParams={{
        redirect_uri: window.location.origin
      }}
    >
      <App />
    </Auth0Provider>
  </React.StrictMode>,
)
```

## Get authenticated user

In your src/pages/Home.tsx file, you use the useAuth0 hook to get the authenticated user. Here's an example of how to use the useAuth0 hook:

```Typescript
import { useAuth0 } from '@auth0/auth0-react';

function Home(): JSX.Element {
    const { user } = useAuth0();
    // use object to display user information...
```