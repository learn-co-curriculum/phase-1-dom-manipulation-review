# DOM Manipulation Review

## Learning Goals

- Define the DOM
- Access DOM elements
- Identify best practices for constructing queries

## Introduction

In this section, you will learn how to use _JavaScript events_ to add or modify
content on a web page. We will learn more about exactly what JavaScript events
are in the next lesson. For now, think of the user taking some kind of action on
a web page and the page updating in some way as a result. Some examples might
include:

- Logging in to your bank's website to see your account balance.
- Submitting a form to register for an event.
- Clicking on an email in a list view to see the email's content.

In each of these cases, the content may be different for different users or on
different days — it's _dynamic_. This is accomplished by accessing and modifying
the _Document Object Model_ (DOM) using JavaScript and the DOM API.

In this lesson, before we move on to events, we'll review what we've learned
about the DOM.

## Defining the DOM

According to [MDN][mdn-dom]:

> The Document Object Model (DOM) is the data representation of the objects that
> comprise the structure and content of a document on the web.

But what exactly does this mean?

When a web page loads, the content that appears on the page initially represents
the underlying HTML and CSS. If the page being loaded includes dynamic content,
however, it doesn't make sense for that content to be reflected in the HTML. If
we're viewing a list of movies playing nearby, it wouldn't be feasible to
include that list in the HTML because the list reflects a particular location,
and will probably change in a few hours or days. Instead, dynamic content is
populated using JavaScript code, and the DOM is what enables this to happen.

Unlike HTML, the DOM doesn't exist as a file on a machine somewhere. It is
instead just an electronic representation of the HTML, CSS and JavaScript used
to create the content a user sees on the page. Most pages will include both some
_static_ elements (for example, the logo, nav bar, search box, etc.) as well as
dynamic content that can change depending on when, where, or by whom it is
accessed. The combination of all of those elements is what appears to a user
when they access the page.

The DOM serves as the middle layer between the user and the HTML/CSS files that
are stored on the site's server.

## Accessing DOM Nodes

Let's consider the example of clicking a button to log in to a web app. In order
to implement that functionality, we need write code that detects when the button
is clicked, then performs whatever actions are needed to log the user in and
display their home page content. In order to be able to do that, the first step
is to access the button in the DOM.

The [Document interface][mdn-document] provides [instance
methods][document-methods] that can be used to access specific elements in the
DOM. We will review a few of the most commonly used methods. If you would like a
more comprehensive review, revisit the Manipulating the DOM section in Software
Engineering Prep.

### Accessing Content Using CSS Selectors

The Document Interface includes two methods that can be used to access content
using [CSS selectors][css-selectors]: [`querySelector()`][query-selector] and
[`querySelectorAll()`][query-selector-all]. Both methods accept a string
representing a CSS selector as an argument. `querySelector()` returns the first
element found that matches the CSS selector, while `querySelectorAll()` returns
a [`NodeList`][nodelist], an array-like collection containing all of the
elements that match the selector.

### Accessing Content Using Metadata

There are also some methods that can be used to access elements with certain
types of metadata. The most common are: [`getElementById()`][get-by-id],
[`getElementsByClass()`][get-by-class], and
[`getElementsByTagName()`][get-by-tag-name]. Each accepts a string representing
the desired value as an argument and locates the matching element or elements.
Because `ID`s are expected to be unique, `getElementById()` returns a single
element, while the other two return an [HTML Collection][html-collection], an
array-like collection containing all elements that match.

### Practice Creating Queries

Let's get some practice using the methods above. Open `index.html` in the
browser, then open the dev tools. Use the console to experiment with building
each of the queries, then check the answer once you're done.

**Note**: there are multiple ways to solve each of them — as long as your query
works, it is correct!

**Challenge #1:** Using the `querySelectorAll()` method, write a query to return
the second step under "Install the pump". Your query should return the `li`
containing the text "Place the pump in the bottom of the bucket".

<details><summary><b>Hint</b></summary><p>
    <ul>
      <li>Remember that <code>querySelectorAll()</code> returns a <em>NodeList</em> that works similarly to a JavaScript array.</li>
    </ul>
</p></details>

<details><summary><b>Answer</b></summary>
    <p>Here's one possible solution:</p>
    <pre>document.querySelectorAll("ol li li")[1];</pre>
    <p>The `querySelector()` returns a list of `li`s that are children of an `li` that is itself the child of an `ol`. The `[1]` pulls out the second element in that list. If it's not clear what's happening, use the console to try it out one step at a time.</p>
</details>

**Challenge #2**: Now construct the same query using the `querySelector()` method.

<details><summary><b>Hint</b></summary><p>
    <ul>
      <li>Check out <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/General_sibling_combinator">sibling selectors</a></li>
    </ul>
</p></details>

<details><summary><b>Answer</b></summary>
    <p>Here's one possible solution:</p>
    <pre>document.querySelector("ol li ol li ~ li");</pre>
    <p>Make sure you understand what's happening here before moving on.</p>
</details>

**Challenge #3**: Using one of the metadata methods, select the "Supplies
needed" unordered list and save it to a variable. Then access the first `li` in
the saved element's _sublist_ and return that `li`'s text content ("Small rocks").

<details><summary><b>Answer</b></summary>
    <p>Here's one possible solution:</p>
    <pre>const supplies = document.getElementById("supplies");
supplies.querySelector("li li").textContent;</pre>
</details>

**Challenge #4**: Modify your query from above to make it one line, without
saving the intermediate result.

<details><summary><b>Hint</b></summary><p>
    <ul>
      <li>Each of the methods we've talked about in this lesson is called on a <em>DOM node</em>. As long as a query returns a DOM node rather than a <em>collection</em>, we can chain a second method to the end of it!</li>
    </ul>
</p></details>

<details><summary><b>Answer</b></summary>
    <p>Here's one possible solution:</p>
    <pre>document.getElementById("supplies").querySelector("li li").textContent;</pre>
</details>

**Challenge #5**: Using one of the metadata methods, select the `ul` under the
Maintenance heading, then select the second list item ("Clean the bath weekly or
as needed") and return its text content. You can do it as a one-line query or
save intermediate results — or try it both ways!

<details><summary><b>Answer</b></summary>
    <p>Here's one possible solution:</p>
    <pre>document.getElementsByClassName("maintenance")[0].querySelectorAll("li")[1].textContent;</pre>
</details>

Before moving on, try a couple of your own!

## Factors in Creating Queries

You may have noticed that there are many different ways to approach constructing
queries — choosing the methods to use, deciding whether to save intermediate
results, etc. There is no "right" way to construct queries, but in general, you
want to try to find a good balance between readability and efficiency.

As with array iterator methods, try to use more expressive methods when you can.
Using `getElementById()`, for example, is more expressive (and therefore more
readable) than using `querySelector()` with a complicated CSS selector.

You don't necessarily need to save intermediate results of your queries, but
recall that complex, one-line queries can lead to code that is difficult to read
and to debug. A good approach is to save an intermediate result to a variable if
you're going to use the result more than once. Using a meaningful variable name
for an element that's going to be used multiple times in your code will help
with readability and also with efficiency, since you won't be repeating the
query multiple times.

Finally, remember that when you get to creating applications of your own, you
can set up your app in a way that maximizes both readability and efficiency. If
you are creating an element that you know will be clickable and therefore will
need an event listener, give it an `id`. Or if you'll need to give multiple
elements the same event listener, give them all the same `class`. And, as
always, use meaningful names for your variables and metadata. Anyone who reads
your code — including you! — will thank you for it.

## Conclusion

In this lesson, we reviewed how to write JavaScript to access elements in the
DOM and gotten some practice creating queries. We are now ready to start
learning about JavaScript events and put our DOM manipulation knowledge to use!

## Resources

- [Introduction to the DOM][mdn-dom]
- [DOM Instance Methods][document-methods]

[mdn-dom]: https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction
[mdn-document]: https://developer.mozilla.org/en-US/docs/Web/API/Document#instance_methods
[document-methods]: https://developer.mozilla.org/en-US/docs/Web/API/Document#instance_methods
[query-selector]: https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector
[query-selector-all]: https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll
[get-by-id]: https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById
[get-by-class]: https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName
[get-by-tag-name]: https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByTagNameNS
[css-selectors]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_Started/Selectors
[nodelist]: https://developer.mozilla.org/en-US/docs/Web/API/NodeList
[html-collection]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection
