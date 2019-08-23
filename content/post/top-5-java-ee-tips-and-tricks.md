---
title: "Top 5 Java EE Tips and Tricks"
author: "amdelamar"
category: "advice"
tags: ["java", "java-ee"]
featured: false
draft: false
date: 2016-06-30T00:00:00.000Z
thumbnail: "https://cdn.ramblingware.com/file/ramblingware/2016/macbook-code-640.jpg"
banner: "https://cdn.ramblingware.com/file/ramblingware/2016/macbook-code-640.jpg"
bannerCaption: "Keep Calm and Keep Coding those beautiful Java EE websites. (Photo Credit: NegativeSpace)"
description: "5 ways you can improve your Java EE web applications, with these tips and tricks."
---

I'll save the obvious "must-have" features for Java EE webapps, for later. This post will focus on the smaller details of Java EE programming, and what useful methods might save you time and possibly increase site traffic. If you've already started building your site, or you're maintaining an old behemoth, then these tips should help tremendously.

Here we go:

## 1. Handle exceptions in JSPs

Make an Exception JSP page, and set error handling inside your other JSP pages.

Add this directive to your existing JSP pages and fragments.

```xml
<!-- [All plain JSP pages] -->
<%@ page errorPage="/jsp/exception.jsp" %>
```

Then make an exception.jsp page with this directive near the top.

```xml
<!-- [exception.jsp] -->
<%@ page isErrorPage="true" %>
<!-- Exception : -->
<p><%= exception.getLocalizedMessage() %></p>
<!-- Stack Trace : -->
<c:forEach var="trace" items="${pageContext.exception.stackTrace}">${trace}<br/></c:forEach>
```

This helps tremendously when debugging bad code inside JSPs. Especially if your pages are heavily fragmented. You can get a better idea of where that pesky NullPointerException occurs. You could get fancy and show the stack trace too, as shown above. But the only tricky part would be with the line numbers because JSP pages are not compiled like regular Java classes. So the line numbers can be much larger than expected if you're including lots of fragments.

## 2. Avoid coding in JSPs as much as possible.

Avoid blocks of code in your JSP as much as possible. Use a tag library like JSTL Core instead.

Avoid something like this:

```xml
<%ArrayList<String> results = request.getAttribute("results");
if(results != null && results.size() > 0) {
    out.print("<ol class=\"results\">");
    for(String item : results) {
        out.print("<li>"+item+"</li>");
    }
    out.print("</ol>");
} else {
    out.print("<p class=\"error\">No results found!</p>");
}%>
```

Do this instead :

```xml
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:set var="results" scope="request" value="${requestScope.results}" />
<c:if test="${not empty results}">
  <ol class="results">
    <c:forEach var="item" items="${results}">
      <li>${item}</li>
    </c:forEach>
  </ol>
</c:if>
<c:else>
  <p class="error">No results found!</p>
</c:else>
```

At first glance, it doesn't seem too different. But the separation of design from code here, is tremendous. Now the IDE or editor you use can readily parse HTML tags, instead of blocking off entire chunks of your JSP pages. And YOU as the developer can focus on the front-end coding, rather than editing Java code every time a CSS or design structure changes.

## 3. Reduce and close as many database connections as possible.

Maybe you have 1 query to gather user statistics. 1 query to get `<select>` options for the form. 1 query to get related items in the side-bar. And finally, 1 query to get the search results. That's 4+ connections and 4+ SQL queries.

If you can reuse the same connection for all 4 queries, that would help. Or, if you can reduce the accessory queries (like the items in the side-bar) to run once, then store the values into the session attribute, then that would reduce the calls per all page loads.

The fewer database calls the better! This reduces the time it takes to execute and return pages. Use a try/catch/finally block to open connections, execute queries, and close connections.

## 4. Fragment reusable JSP portions as much as possible.

Yeah, you know that hoverable menu with submenus on all those pages?  
Move it to a separate "menu.jspf" page by itself.

Make it reusable, and encapsulated. It makes your pages much more maintainable when those big client requirements need to be done.

Use static `<%@ include` and dynamic `<jsp:include` for certain page fragments. The difference here, is dynamic include is compiled at runtime. While static include is merely copied from the source to the target JSP, and treated as one page.

Static include means they are put inside the calling page, and executed as one request. Dynamic include means each page is executed separately as their own request. Learn to use the right include for the right portion of your pages.

For example, use static includes on simple fragments, like a footer. Something that doesn't change. Save the dynamic includes for menus, tabs, and side-bar content.

## 5. Store page-specific attributes in the servlet response, and user-specific attributes in the servlet session.

```java
// Request attribute
HttpServletRequest.setAttribute("message","The email was sent!");
// Session attribute
HttpServletRequest.getSession().setAttribute("username", user.getName());
```

Learn to store page variables like search results, into the request. And store user related variables in the session. That way, if a user navigates away from the search page, for example. Then they still retain their session, but the request page that produced the search results is no longer needed.

Requests are shorter, single-use. Sessions are longer, multi-use.

Also, keeping objects in their respective scopes reduces overhead, which helps performance in general.

And there you have it! I hope to come up with a basic guide in the future to help prioritize new Java EE projects with important features and tid-bits. But for now this should help; especially to existing monster projects.

And if your project already make use of these five tips, then you're well on your way to polishing off and deploying your web application! Good luck!
