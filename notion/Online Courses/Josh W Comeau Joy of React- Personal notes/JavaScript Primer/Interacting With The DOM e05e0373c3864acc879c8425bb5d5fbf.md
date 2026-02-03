# Interacting With The DOM

Appears in [**Build Your Own React**](Module%201%20React%20Fundamentals%2071ed585645214bd290aaea0016a19c76.md)

Most of the methods we've seen, like `querySelector` and `appendChild`, can be called on any DOM node. `createElement` is different: it can only be called on the `document` object.

In order for a DOM node to be visible to the user, it needs to be within the `<body>` tag. The user won't see any HTML tags in other parts of the page. And so when we create an element, it's associated with the document, but it won't be visible until we append it somewhere within the `<body>`.