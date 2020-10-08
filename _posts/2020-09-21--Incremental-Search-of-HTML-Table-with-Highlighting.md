---
layout: post
title: "Incremental Search of HTML Table with Highlighting"
date: 2020-09-21
categories: javascript programming
---

**TL;DR:** Please try out a **live, working example** on jsfiddle: [Incremental Search of HTML Table with Highlighting](https://jsfiddle.net/jftuga/L3oxtsz2/19/)

I wanted a way to enhance our internal phone extension list.  Another department manages an Excel spreadsheet with this information. They then publish it to SharePoint Online as a non-searchable PDF.  One goal of this format is that all 200+ names should be able to be printed on a single page. The spreadsheet looks something like this:

Last, First | Extension | Dept | Last, First | Extension | Dept | Last, First | Extension | Dept 
------------|-----------|------|-------------|-----------|------|-------------|-----------|-----
Bean, Andrea | 9-0001 | Acct | Decker, Ashley | 8-0001 | HR  | Hunter, Annabel | 9-0002 | Acct
Berman, Ash  | 8-0002 | HR   | Denton, Andy   | 9-0003 | Ops | Hunt, Ethan     | 8-0003 | Ops

___


It prints great!  However, using it with SharePoint Online is a less than optimal experience.  By default, the PDF renders so small it is unreadable.  The first chore an employee has to do is to enlarge it to 150% in order to read any of the names or numbers.

I wanted to improve the user experience yet also not make the document maintainers have to change their process.  I was looking for a WIN-WIN-WIN - more on the third WIN later.

The first step was to use some `Python` code along with the `openpyxl` library which can be used to read and write `Excel` spreadsheets.

The function below reads data from the Excel spreadsheet and returns a sorted `list` of Python strings, with each `string` containing: `name;extension;dept`

{% highlight python %}
    def get_numbers_from_excel(self):
        """ read all rows and columns of the Excel spredsheet defined by 'sheet'
            'self.phone_data' is a list that will be populated with delimited strings
            in this format: {name};{extension};{dept}
            conveniently, each string starts with a person's last name, which is how
            the list should be sorted
        """
        wb = openpyxl.load_workbook(filename = self.source_xlsx, read_only=True)
        sheet = wb["Sheet1"]
        for row in range(1,sheet.max_row):
            for column in range(1,sheet.max_column,3):
                cell=sheet.cell(row=row, column=column)
                if not cell.value:
                    continue
                if row > 200:
                    break

                name = cell.value
                cell = sheet.cell(row=row, column=column+1)
                dept = cell.value
                cell = sheet.cell(row=row, column=column+2)
                number = cell.value
                if not (name and dept and number):
                    continue

                entry = f'"{name};{number};{dept}"'
                self.phone_data.append(entry)

        self.phone_data.sort()
{% endhighlight %}

Now that the data is ready, how can it be processed by Javascript?  I found this jsfiddle: [Incremental Search without Library](https://jsfiddle.net/bc_rikko/67ovedcm/).  This example works great, but it is
for a `list` and not a `table`. I also want to be able to incrementally search any of the three columns, not just name. Highlight the search term in each result will also be included.

Initally, this code will display all 200+ entries.  As you start typing, these entries are whittled down via your search phrase.

{% highlight html %}
<b>Search:</b> <input type='text' id="keyword">
<ul id="list"></ul><br />
<TABLE id="dataTable" border="1" cellspacing="3" cellpadding="3">
<THEAD><TH>Name</TH><TH>Number</TH><TH>Department</TH></THEAD></TABLE>

<script type="text/javascript">//<![CDATA[
var data = [ __PHONE_DATA__ ]

function clearTable(table) {
    for(var i = table.rows.length - 1; i > 0; i--) {
      table.deleteRow(i);
    }
}

function buildCell(row, entry, searchQuery) {
  var cell = row.insertCell(-1);
  highlight = new RegExp(searchQuery ,"i");

  if(searchQuery) {
    cell.innerHTML = entry.replace(highlight, "<mark>" + "$&" + "</mark>");
  } else {
    cell.appendChild(document.createTextNode(entry));
  }
}

function buildTable(tableID, data, searchQuery) {
    var table = document.getElementById(tableID);
    clearTable(table)
    for(i=0; i < data.length; i++) {
        var row = table.insertRow(-1);
        slots = data[i].split(";");
        for(j=0; j<3; j++) {
          buildCell(row,slots[j],searchQuery);
        }
    }
}

buildTable("dataTable", data, "")

var keyupStack = [];
var keyword = document.getElementById('keyword');
keyword.addEventListener('keyup', function () {
    keyupStack.push(1);
    
    setTimeout(function () {
      keyupStack.pop();
      if (keyupStack.length === 0) {
        var buf = '.*?' + this.value.replace(/(.)/g, "$1");
        var reg = new RegExp(buf,'i');

        var filteredLists = data.filter(function (d) {
          return reg.test(d);
        });

        buildTable("dataTable",filteredLists,this.value); 
      }
    }.bind(this), 300);
});
//]]></script>
{% endhighlight %}

The Python `list` containing the employee information gets pushed into the above HTML template where the `__PHONE_DATA__` place holder resides.  Since phone numbers are only updated once a day, I regenerate this HTML file each night via a Windows Schedule Task.

Please try out a **live, working example** on jsfiddle: [Incremental Search of HTML Table with Highlighting](https://jsfiddle.net/jftuga/L3oxtsz2/19/)

___

##  Conclusion

* Win #1 - The spreadsheet maintainers do not have to change their input method.
* Win #2 - Employees can now more easily find internal names, phone numbers, and departments.
* * If they happen to know regular expressions, they can use them in the search box!
* **Bonus Win** - Now that I have each employee name and number, I can match their name in `Active Directory` and then populate their `OfficePhone` attribute with their phone number.
* * This could also be done with the `Department` attribute.
