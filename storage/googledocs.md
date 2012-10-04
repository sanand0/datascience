# Google Docs

[Google Docs](https://docs.google.com/) -- which has now become [Google
Drive](https://drive.google.com/) lets you store spreadsheets online, and
access the data via programs. This allows you to:

- [Upload your spreadsheets](http://support.google.com/docs/bin/answer.py?answer=1250382)
  and [folders](http://support.google.com/docs/bin/answer.py?answer=1250384)
- [Share files](http://support.google.com/drive/bin/answer.py?answer=2494822)
  and [folders](http://support.google.com/drive/bin/answer.py?answer=2375030) privately
  or publicly
- [Create charts](http://support.google.com/docs/bin/answer.py?answer=63728)
  and [visualisations](https://developers.google.com/chart/interactive/docs/spreadsheets)
- Expose an [API](https://developers.google.com/google-apps/spreadsheets/) to the data

You can [store up to](http://support.google.com/docs/bin/answer.py?answer=2505921) 400,000 cells and 256 columns.

The Guardian [uses Google Docs](http://www.niemanlab.org/2010/08/how-the-guardian-is-pioneering-data-journalism-with-free-tools/)
to store and share most of its [public data](http://www.guardian.co.uk/data).

## Google Spreadsheets as a tool

In addition to standard spreadsheet functions, Google Spreadsheets has a number of
[new formulas](http://support.google.com/docs/bin/static.py?topic=25273&page=table.cs&tab=1240289)
that are useful in data exploration.

- You can [import HTML tables and lists](http://support.google.com/docs/bin/answer.py?answer=155182).
  Here are some ways it can be used with:
    - [Weather data](http://tins.rklau.com/2012/07/importhtml-and-google-spreadsheets.html)
    - [Olympics medals](http://mashe.hawksey.info/2012/05/hacking-stuff-together-with-google-spreadsheets-using-importhtml-to-create-a-winter-olympics-2010-medal-map/)
    - [Internet traffic](http://rud.is/b/2012/01/13/importhtml/)

  It is also possible to use it with other Google Docs formulas to create sophisticated results:
    - [Demographics data](http://mashe.hawksey.info/2012/09/reshaping-importhtml-data-in-google-spreadsheet-using-query-and-transpose-formula/)
    - [Election results](http://blog.ouseful.info/2010/03/11/screenscraping-with-google-spreadsheets-app-script-and-the-importhtml-formula/)

- You can get [financial information](http://support.google.com/docs/bin/answer.py?answer=155178)
  directly from Google Finance. This includes historical stock prices, fund NAVs, etc.

- [Query spreadsheets](http://support.google.com/docs/bin/answer.py?answer=1388882) like a database

- You can [translate text](http://support.google.com/docs/bin/answer.py?answer=1388877) in the cells

- Insert [images](http://support.google.com/docs/bin/answer.py?answer=1388859) which could be
  [image charts](https://developers.google.com/chart/image/). (Image charts will not work for long though.)

- [Create charts](http://support.google.com/docs/bin/answer.py?answer=63728)
  and [visualisations](https://developers.google.com/chart/interactive/docs/spreadsheets)

## Google Spreadsheets API

Google Spreadsheets can be used as a data source using an [API](https://developers.google.com/google-apps/spreadsheets/).

For examples, take the [Hacker News Cofounder List](https://docs.google.com/spreadsheet/ccc?key=0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE&hl=en) spreadsheet
at https://docs.google.com/spreadsheet/ccc?key=0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE&hl=en

The long string in the middle, `0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE`, is the document key.

You can retrieve the list of worksheets inside by retrieving the URL:

    curl http://spreadsheets.google.com/feeds/worksheets/0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE/public/basic

    {
      "version": "1.0",
      "encoding": "UTF-8",
      "feed": {
        ...
        "entry": [
          {
            "title": {
              "type": "text",
              "$t": "Co-Founder Wish List Good"
            },
            "updated": {
              "$t": "2012-07-29T21:56:00.124Z"
            },
            ...
            "link": [
              {
                "rel": "http://schemas.google.com/spreadsheets/2006#listfeed",
                "type": "application/atom+xml",
                "href": "https://spreadsheets.google.com/feeds/list/0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE/ocz/public/basic"
              },
              {
                "rel": "http://schemas.google.com/spreadsheets/2006#cellsfeed",
                "type": "application/atom+xml",
                "href": "https://spreadsheets.google.com/feeds/cells/0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE/ocz/public/basic"
              },
              {
                "rel": "http://schemas.google.com/visualization/2008#visualizationApi",
                "type": "application/atom+xml",
                "href": "https://spreadsheets.google.com/tq?key=0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE&sheet=ocz&pub=1"
              },
              {
                "rel": "self",
                "type": "application/atom+xml",
                "href": "http://spreadsheets.google.com/feeds/worksheets/0AgCvDTyBjHdOdDFfMENqeWVGNVFxTXdnaDZBRkd0cUE/public/basic/ocz"
              }
            ]
          },
          {
              ... more sheets ...

Each of the `href`s in the list gives you access to the contents of the data.

This can be used directly in a web page. If you specify `?alt=json-in-script&callback=function_name` as parameters,
the response will be in [JSON-P](http://en.wikipedia.org/wiki/JSONP) that can be embedded into a page.

Here are some examples of how to use the API:

- [Write to a Google Spreadsheet from a Python script](http://www.mattcutts.com/blog/write-google-spreadsheet-from-python/)
- [Using Google Spreadsheets like a Database - The Query Formula](http://blog.ouseful.info/2010/01/19/using-google-spreadsheets-like-a-database-the-query-formula/)
- [Using Google Spreadsheets as a Database with the Google Visualisation API Query Language](http://blog.ouseful.info/2009/05/18/using-google-spreadsheets-as-a-databace-with-the-google-visualisation-api-query-language/)
- [Google Spreadsheets API: Listing Individual Spreadsheet Sheets in R](http://blog.ouseful.info/2011/09/07/google-spreadsheets-api-listing-individual-spreadsheet-sheets-in-r/)

More examples are at [Tony Hirst's blog](http://blog.ouseful.info/tag/google-spreadsheet/)
