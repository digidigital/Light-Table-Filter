# (Enhanced) Light-Table-Filter

A light javascript-only table filter based on: https://codepen.io/chriscoyier/pen/tIuBL / https://codepen.io/oceatoon/pen/jKqEI

Features:

- Supports multiple space-delimited filter phrases
- Can be used with multiple tables on the same page.
- "Include Filter" - Table rows that contain at least one include-phrase are displayed. 
- "Exclude Filter" - Table rows that are visible after the input filter was applied and contain at least one exclude-phrase will be hidden. 

To use it with more than one table just replace "order-table" in the input elements' data-table attributes and the tables's class attribute with a unique value for each table.

## Javascript

```javascript
(function(document) {
    'use strict';

    var LightTableFilter = (function(Arr) {

        var _input;
        var _excludeinput;

        function _onInputEvent(e) {

            var _datatable = e.target.getAttribute('data-table');

            if (e.target.className === 'light-table-filter') {
                _input = e.target;
                _excludeinput = document.querySelector("input[class=light-table-exclude-filter][data-table=" + CSS.escape(_datatable) + "]");
            } else {
                _input = document.querySelector("input[class=light-table-filter][data-table=" + CSS.escape(_datatable) + "]");
                _excludeinput = e.target;
            };

            var tables = document.getElementsByClassName(_input.getAttribute('data-table'));
            Arr.forEach.call(tables, function(table) {
                Arr.forEach.call(table.tBodies, function(tbody) {
                    Arr.forEach.call(tbody.rows, _filter);
                });
            });
        }

        function _filter(row) {
            var text = row.textContent.toLowerCase(),
                val = _input.value.toLowerCase(),
                negval = _excludeinput.value.toLowerCase();

            row.style.display = val.replace(/\s/g, '') == '' ? 'table-row' : 'none';
            val.split(' ').forEach(function(word) {
                row.style.display = text.indexOf(word) === -1 || word == '' ? row.style.display : 'table-row';
            });

            negval.split(' ').forEach(function(word) {
                row.style.display = text.indexOf(word) === -1 || word == '' ? row.style.display : 'none';
            });
        }

        return {
            init: function() {
                var inputs = document.getElementsByClassName('light-table-filter');
                Arr.forEach.call(inputs, function(input) {
                    input.oninput = _onInputEvent;
                });
                var inputs = document.getElementsByClassName('light-table-exclude-filter');
                Arr.forEach.call(inputs, function(input) {
                    input.oninput = _onInputEvent;
                });
            }
        };
    })(Array.prototype);

    document.addEventListener('readystatechange', function() {
        if (document.readyState === 'complete') {
            LightTableFilter.init();
        }
    });

})(document);
```
## HTML
```html
	<h2>Light Javascript Table Filter</h2>

	<input type="search" class="light-table-filter" data-table="order-table" placeholder="Include-Filter">
	<input type="search" class="light-table-exclude-filter" data-table="order-table" placeholder="Exclude-Filter">
	<table class="order-table table">
		<thead>
			<tr>
				<th>Name</th>
				<th>Email</th>
				<th>Phone</th>
				<th>Price</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>John Doe</td>
				<td>john.doe@gmail.com</td>
				<td>0123456789</td>
				<td>99</td>
			</tr>
			<tr>
				<td>Jane Vanda</td>
				<td>jane@vanda.org</td>
				<td>9876543210</td>
				<td>349</td>
			</tr>
			<tr>
				<td>Alferd Penyworth</td>
				<td>alfred@batman.com</td>
				<td>6754328901</td>
				<td>199</td>
			</tr>
		</tbody>
	</table>
	<br/>
	<input type="search" class="light-table-filter" data-table="another-table" placeholder="Include-Filter">
	<input type="search" class="light-table-exclude-filter" data-table="another-table" placeholder="Exclude-Filter">
	<table class="another-table table">
		<thead>
			<tr>
				<th>Part Name</th>
				<th>Part-Number</th>
				<th>Location</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<td>Tyre</td>
				<td>DZ-1234-56789</td>
				<td>Berlin</td>
			</tr>
			<tr>
				<td>Door</td>
				<td>DD-8765-43210</td>
				<td>Paris</td>
			</tr>
			<tr>
				<td>Motor</td>
				<td>ZZ-6754-38901</td>
				<td>New York</td>
			</tr>
						<tr>
				<td>Motor</td>
				<td>ZZ-6754-12354</td>
				<td>Berlin</td>
			</tr>
			<tr>
				<td>Spare Tyre</td>
				<td>DZ-1234-99989</td>
				<td>Paris</td>
			</tr>
		</tbody>
	</table>
```
