---
layout: post
title:      "Submitting Images (and non-string form data) via AJAX"
date:       2018-02-10 17:37:41 +0000
permalink:  submitting_images_and_non-string_form_data_via_ajax
---


In a recent project, I changed my create action to submit as a post with AJAX, rather than a traditional rails create with a page a refresh.  For the most part this was a simple change.  In my javascript, I did the following:


```
$('#new_item').submit(function(e){
	e.preventDefault();
	var url = '/items'
	var params = $(this).serialize();
	$.ajax({
			type: "POST",
			url: url,
			data: params
		});
});
```

I had an active model serializer set up for my Item model, so using the jQuery .serialize() was all I needed to do to turn my form data into a nice JSON string for the post.  It was almost too easy.  Next, I realized that I was going to also need to submit images with my form, which presented a problem.  Uploading images didn't work with jQuery's .serialize(), so I needed another solution to submit my form data with AJAX.  (FYI - I am using [paperclip](https://github.com/thoughtbot/paperclip) for image handling in my Rails application).  I did some googling and luckily found[ this](https://stackoverflow.com/questions/24268931/uploading-image-by-ajax-cannot-use-the-serialize-method-in-jquery) wonderful post on stackoverflow.  I changed my code and it worked!  Here's my updated code below:

```
$('#new_item').submit(function(e){
	e.preventDefault();
	var url = '/items'
	var formData = new FormData(this);
	$.ajax({
			type: "POST",
			url: url,
			data: formData,
			contentType: false,
			processData: false
		});
});
```

Making use of FormData() allowed me to keep my uploaded image in my post data.  Then, adding the additional params of `contentType: false` and `processData: false` allowed me to submit an image with my form data.  From the jQuery .ajax() [documentation](http://api.jquery.com/jquery.ajax/), I read:

"contentType (default: 'application/x-www-form-urlencoded; charset=UTF-8')
Type: Boolean or String
When sending data to the server, use this content type. Default is "application/x-www-form-urlencoded; charset=UTF-8", which is fine for most cases. If you explicitly pass in a content-type to  $.ajax(), then it is always sent to the server (even if no data is sent). As of jQuery 1.6 you can pass false to tell jQuery to not set any content type header. Note: The W3C XMLHttpRequest specification dictates that the charset is always UTF-8; specifying another charset will not force the browser to change the encoding. Note: For cross-domain requests, setting the content type to anything other than application/x-www-form-urlencoded, multipart/form-data, or text/plain will trigger the browser to send a preflight OPTIONS request to the server."

and 

"processData (default: true)
Type: Boolean
By default, data passed in to the data option as an object (technically, anything other than a string) will be processed and transformed into a query string, fitting to the default content-type "application/x-www-form-urlencoded". If you want to send a DOMDocument, or other non-processed data, set this option to false."
