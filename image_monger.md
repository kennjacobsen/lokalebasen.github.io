---
layout: provider_api
---
## Image Monger

Lokalebasen.dk does not currently support uploading floor plans or photos using
the PDF format. To facilitate integrating with Lokalebasen.dk for providers that
store floor plans or photos in the PDF format, we have developed a small
webservice that can convert PDF files to JPEG in a single request. Each PDF page
is served individually.

### How does it work?

ImageMonger takes a URL to a PDF-file as an argument. Make sure the PDF file
is publicly accessible.

ImageMonger is available at http://imagemonger.lokalebasen.dk/pdf/{file_url}/{action}

The PDF URL's must be URL-encoded. E.g.

<pre>
  http://dev.lokalebasen-uploads.dk/example.pdf
</pre>

becomes

<pre>
  http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf
</pre>

### Pagelist
To get a list of pages, use the menu action.

<pre>
  http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/menu
</pre>

This will return an <code>application/json</code> response containing an array
of strings. Each string represents a URL to the designated page.

<pre>
[
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/1",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/2",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/3",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/4",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/5",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/6",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/7",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/8",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/9",
   "http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/10"
]
</pre>

#### Return codes
* 200 OK
* 404 Resource Not Found

### Individual pages

To get a JPEG version of a specific page, GET the matching URL. E.g.

<pre>
  http://imagemonger.lokalebasen.dk/pdf/http%3A%2F%2Fdev.lokalebasen-uploads.dk%2Fexample.pdf/4
</pre>

This will return an <code>image/jpeg</code> response on succesful requests
and a <code>text/plain</code> response on failed requests.

#### Return codes
* 200 OK
* 404 Resource Not Found

### Performance notes

As this service downloads and converts/analyzes the specified PDF file on every
HTTP request, please use it with care. Avoid using it for excessively large files
(> 50 MB) and if used in loops, please wait a few seconds between each request
to avoid exhausting the capacity.
