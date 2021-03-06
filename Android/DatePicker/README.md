1. Copy the .js file to your 'www' folder
2. Create a package com.phonegap.plugin
3. Copy the .java file into this package
4. Update your res/xml/plugins.xml file with the following line:

   `<plugin name="DatePickerPlugin" value="com.phonegap.plugin.DatePickerPlugin"/>`

5. Add a class="nativedatepicker" to your <input> element for which you want the native datepicker
6. Add the following jQuery fragment to handle the click on these input elements:

```
	$('.nativedatepicker').click(function(event) {
		var currentField = $(this);
		var myNewDate = Date.parse(currentField.val()) || new Date();

		// Same handling for iPhone and Android
		window.plugins.datePicker.show({
			date : myNewDate,
			mode : 'date', // date or time or blank for both
			allowOldDates : true
		}, function(returnDate) {
			var newDate = new Date(returnDate);
			currentField.val(newDate.toString("dd/MMM/yyyy"));
			
			// This fixes the problem you mention at the bottom of this script with it not working a second/third time around, because it is in focus.
			currentField.blur();
		});
	});

	$('.nativetimepicker').click(function(event) {
		var currentField = $(this);
		var time = currentField.val();
		var myNewTime = new Date();

		myNewTime.setHours(time.substr(0, 2));
		myNewTime.setMinutes(time.substr(3, 2));

		// Same handling for iPhone and Android
		plugins.datePicker.show({
			date : myNewTime,
			mode : 'time', // date or time or blank for both
			allowOldDates : true
		}, function(returnDate) {
			var newDate = new Date(returnDate);
			currentField.val(newDate.toString("HH:mm"));
		});
	});
```

7. It is recommended to prefil these input fields and make these fields read only with a standard date format, preferably with three letter month so it can always be parsed.

8. You may need to convert the result of date.parse() back to an object to get the picker to work a second or third time around. If so, try inserting this code after the myNewDate declaration:

``` 	if(typeof myNewDate === "number"){
		myNewDate = new Date (myNewDate);
	}
	```