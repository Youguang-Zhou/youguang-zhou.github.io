---
title: 'React Learning: Router'
date: 2020-06-08 23:11:14
tags:
- React
categories:
- [React, Notes]
---

*React is a mature web framework developed by facebook.*

----------------------------------------

# Installation

    npm install react-router-dom

# Primary Components
- routers, like `<BrowserRouter>` and `<HashRouter>`
- route matchers, like `<Route>` and `<Switch>`
- and navigation, like `<Link>`, `<NavLink>`, and `<Redirect>`

# `<BrowserRouter>` and `<HashRouter>`
BrowserRouter requires the server to be configured correctly, otherwise will show 404 not found page.

Usage is to wrap the top-level `<App>` element in a router:

    ReactDOM.render(
        <BrowserRouter>
            <App />
        </BrowserRouter>,
        document.getElementById("root")
    );

# Route Matchers
There are two route matching components: `Switch` and `Route`:

    function App() {
        return (
            <Switch>
                {/* If the current URL is /about, this route is rendered
                    while the rest are ignored */}
                <Route path="/about">
                    <About />
                </Route>
                {/* Note how these two routes are ordered. The more specific
                    path="/contact/:id" comes before path="/contact" so that
                    route will render when viewing an individual contact */}
                <Route path="/contact/:id">
                    <Contact />
                </Route>
                <Route path="/contact">
                    <AllContacts />
                </Route>
                {/* If none of the previous routes render anything,
                    this route acts as a fallback.
                    Important: A route with path="/" will *always* match
                    the URL because all URLs begin with a /. So that's
                    why we put this one last of all */}
                <Route path="/">
                    <Home />
                </Route>
            </Switch>
        );
    }

- `strict` matching:

        <Route strict path="/one/">
            <About />
        </Route>

    |  path   |  location.pathname | matches |
    |---------|--------------------|---------|
    |  /one/  |   /one             |   no    |
    |  /one/  |   /one/            |   yes   |
    |  /one/  |   /one/two         |   yes   |

- `exact` and `strict` matching:

        <Route exact strict path="/one">
            <About />
        </Route>

    |  path   |  location.pathname | matches |
    |---------|--------------------|---------|
    |  /one   |   /one             |   yes   |
    |  /one   |   /one/            |   no    |
    |  /one   |   /one/two         |   no    |

# Navigation
The difference between `<Link>` and `<NavLink>` is that `<NavLink>` has an "active" property.

    <Link to="/">Home</Link>
    // <a href="/">Home</a>

    <NavLink to="/react" activeClassName="hurray">
        React
    </NavLink>

    <Redirect to="/login" />

### Check React Router documentation for more details: https://reacttraining.com/react-router/web/guides/quick-start