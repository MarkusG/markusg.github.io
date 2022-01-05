---
layout: post
title: "Using C#'s Anonymous Types to Keep My JSON Lean"
date: 2021-10-12
draft: false
---

My most recent project uses ASP.NET Core's Razor pages its web app functionality. It uses Entity Framework for data storage, and I decided to use [d3](https://d3js.org/) to visualize some of that data. This led to the question of how to get the data from my C# code to the JavaScript code that calls d3. As it turns out, the solution is quite simple. Say there's some property `Datapoints` on the page's viewmodel. One could pass this data to JavaScript as follows:
```html
<script type="text/javascript">
const data = @Html.Raw(JsonSerializer.Serialize(Model.Datapoints));
// fun d3 stuff
</script>
```

This works in principle, but these are Entity Framework models - navigation properties and all. I quickly ran into object cycles when trying to serialize these models, and needed a way to ignore their navigation properties.

### The Bad Solution

The problem was that navigation properties were causing object cycles. Very well, let's set them to `null` and they won't be serialized. Furthermore, using JSON.NET's `NullValueHandling` option can have the nullified properties be ignore entirely during serialization!
```html
<script type="text/javascript">
const data = @Html.Raw(JsonSerializer.Serialize(
	Model.Datapoints.Select(x => {
		x.NavigationProperty1 = null;
		x.NavigationProperty2 = null;
		return x}),
	new JsonSerializerSettings { NullValueHandling = NullValueHandling.Ignore }));
// fun d3 stuff
</script>
```

This works, but it can be better. Firstly, the length of our selector grows as the number of *unwanted* properties grows. In the same vein, we're still serializing every other property of each data point. Ideally, we want to only serialize the properties that are relevant to our visualizations, which both avoids object cycles and slims down the size of our page.

### Enter Anonymous Types

By selecting a new object for each data point, we can pick and choose exactly which properties we want to include in our serialized data:
```html
<script type="text/javascript">
const data = @Html.Raw(JsonSerializer.Serialize(
	Model.Datapoints.Select(x => new {
		IndependentVariable = x.SomeProperty,
		DependentVariable = x.OtherProperty
	}));
// fun d3 stuff
</script>
```

Combining LINQ's `Select()` method with anonymous types has proven to be an incredibly flexible tool for transforming collections of data. In this case, it helped cut down on webpage size, but there are countless other ways in which it can be useful.

### Final Remarks

One piece of general knowledge I've gained from this process is to only transmit the data that needs to be transmitted. I initially started using `Select()` with anonymous types to avoid object cycles, but I've since converted every serializtion of this kind to use it. It cuts down on page size and clarifies which properties are relevant to the data visualization.

Also, every piece of code in this article [smells](https://www.wikiwand.com/en/Code_smell). A C# LINQ query inside a JavaScript script inside an HTML page hurts to look at. I've since started rewriting this app's frontend in [Blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor), which has me really excited.
