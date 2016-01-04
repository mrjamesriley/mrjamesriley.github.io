---
layout: post
title: "Easy HTML Table Pagination and Sorting with DataTables"
date: 2011-12-23 23:53
categories: blog
---

[DataTables](http://datatables.net) is a powerful jQuery plugin that easily turns a standard HTML table into one that is sortable,
paginatable, searchable and
highly customisable. Having used the plugin on multiple occasions over the past few months, I deem it a huge time saver and if it's
interactive tables you're after – I wouldn't look elsewhere. The
[basic example usage](http://datatables.net/release-datatables/examples/basic_init/zero_config.html) makes its immediate power clear and there's a wealth of options, an API and plenty of examples to satisfy most cases. Here's a few tips on how to handle a few of the requirements I
commonly had. I love and use CoffeeScript but for your convenience, the examples below make use of the compiled Javascript,
simplified in places.

<!--more-->

### Custom pagination controls and info

Via the sDom property on DataTable initialisation, it's possible to customise the positioning of the pagination information and controls.
Aside from finding the resulting code hard to read, I also wanted more flexibility. First up, the pagination info (eg. Showing 1-10 of 25).
Upon DataTables creation, you can pass in a function for the 'fnInfoCallback' which is triggered when the related status changes,
as a result of a page change or filter being applied. Here I simply form a string using the available parameters then update the text for the element with the id '#paginationInfo' on the page.

{% highlight javascript %}
"fnInfoCallback": function(oSettings, iStart, iEnd, iMax, iTotal, sPre) {
  info = "Showing " + iStart + "-" + iEnd + " of " + iTotal;
  $('#paginationInfo).text(info);
  return info;
}
{% endhighlight %}

Next I wanted the freedom to position my 'next' and 'previous' pagination buttons anywhere, having assigned the DataTable initialisation to a variable (eg. `userList = $('table#userList').dataTables()`), making use of the public API,
specifically the fnPageChange function of the plugin makes this easy...

{% highlight javascript %}
$('#nextPage').click(function() {
  return userList.fnPageChange('next');
});
$('#prevPage').click(function() {
  return userList.fnPageChange('previous');
});
{% endhighlight %}

### Multi filters and being able to clear all

The DataTables [API](http://datatables.net/api) makes filtering pretty simple with use of the `fnFilter` function, passing a string to filter
on and optionally, a specific column to limit filtering to. With multiple columns having filters applied, the easy way to clear all filters
is to simply call `fnFilter('')` - with an empty string and no column int arguement.

For the search field (which filters the table based on the entered text), I'm using the search input, part of HTML5
`<input id="players-search" placeholder="Search by name..." type="search">` hooked up to filter the table on keyup.
If you're using Webkit, you'll see the search box includes a button on the right to clear the contents of the text field.
To have the table filter cleared when  clicking the 'X': set a 'click' event on the search input which then takes its value
and filters accordingly. In this case, the value will now be blank after having clicked the 'X' which effectively clears filtering on the table.

### Using images according to the row data

It's not long untill you'll find yourself wanting to add buttons or images to the table, whether it be a users avatar,
or perhaps a 'Buy' button for each product in the table. The `fnRowCallback` allows you to modify each row on draw and it's
here you have access to the data for each column and able to modify the output. Say you have a table of forum users with
the following data, what if you wanted to show an image as a label for those who are admins?

{% highlight html %}
<tr>
  <td>John</td>
  <td>Admin</td>
  <td>London</td>
</tr>
<tr>
  <td>Paul</td>
  <td>Moderator</td>
  <td>New York</td>
</tr>
{% endhighlight %}

When creating the DataTable, use the 'aoColumns' table (or the `aoColumnDefs` alternative) to give classes to the columns to
allow us to easily target each:

{% highlight javascript %}
'aoColumns': [
  { "sClass": "name", },
  { "sClass": "role" },
  { "sClass": "city" }
]
{% endhighlight %}

The nRow parameter of the `fnRowCallback` function refers to the HTML row currently being drawn. In this case, if the column text happens to be
'admin', replace the cells content with the jQuery generated img element:

{% highlight javascript %}
"fnRowCallback": function(nRow, aData, iDisplayIndex, iDisplayIndexFull) {
  role = $('.role', nRow).text();
  if (role == 'admin') {
    adminLabel = $('<img/>', { "class": "admin-icon" })
    $('.role', nRow).html(adminLabel);
  }
  return nRow;
}
{% endhighlight %}

If you wanted to be able to filter on this column too, one solution is to use the 'text-indent' property to visually hide
text, leaving the image all that's visible while still giving DataTables something to work with.

So the purpose of this post was to bring DataTables to your attention and to demonstrate how I've approached implementing the
functionality I desired. For many of your uses, the plugin will be plug and play – and you no longer have an excuse not to provide
these useful table interaction controls for your users.

