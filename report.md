# Bug fixing report

## Metodology

 1. The first problem i fased was setup.py. 
 When I called command 'rss-reader', it showed such error:
 ```sh
No module named 'cache'
```
if you call rss-reader 'directly'(as you see below),there is no such error:
 ```sh
python3 rss_reader.py https://news.yahoo.com/rss/ --limit 1
```
I found soluthion there:
https://stackoverflow.com/questions/24722212/python-cant-find-module-in-the-same-folder
I've just added to rss_reader.py these comands. And it works!
 ```sh
file_dir = os.path.dirname(__file__)
sys.path.append(file_dir)
```
2. Three other problems were syntax:

There was incorrect import of function ***output_json***  in 'rss_reader.py'
**Before:**
 ```sh
from cmd_line_parser import make_arg_parser, outputjson, output_verbose
```
**After:**
 ```sh
from cmd_line_parser import make_arg_parser, output_json, output_verbose
```

There was incorrect positions of code blocks in 'validator.py':

**Before:**
 ```sh
if cmd_args.source:
        url = cmd_args.source
        try:
            requests.get(url)
            LOGGER.info('Check the URL availability.')
    except Exception:
        raise er.UnreachableURLError("URL is invalid.")
```
**After:**
 ```sh
    if cmd_args.source:
        url = cmd_args.source
        try:
            requests.get(url)
            LOGGER.info('Check the URL availability.')
        except Exception:
            raise er.UnreachableURLError("URL is invalid.")
```
There was missing symbol in 'cmd_line_parser.py'.

**Before:**
 ```sh
 parser.add_argument('--to-html', type=str, default=',
                    help='Converts news in html format. Receives the path for file saving')
```
**After:**
 ```sh
    parser.add_argument('--to-html', type=str, default='',
                        help='Converts news in html format. Receives the path for file saving')
```

3. The other problem apperied while I was trying to run example from READ.me
 ```sh
AttributeError: object has no attribute 'description'
```
It was problem in 'rss_parser.py'. I have fixed it such way:
**Before:**
 ```sh
info_description = info.description
```
**After:**
 ```sh
info_description = getattr(info, 'description', '')
```
## Testing
 ```sh
(.venv) (base) MacBook-Air-Sasa:rss_reader sandrayakimova$ python -m unittest discover tests/
.............
----------------------------------------------------------------------
Ran 13 tests in 0.130s

OK
```

Lets try to run some commands from our task:

**input:**
 ```sh
rss-reader https://techcrunch.com/feed/ --limit 1  
```

**output:**
 ```sh
Feed: TechCrunch

Title: QOA brings in seed round to do for chocolate what Oatly did for milk

Date: Wed, 27 Oct 2021 10:00:15 +0000
Link: https://techcrunch.com/2021/10/27/qoa-brings-in-seed-round-to-do-for-chocolate-what-oatly-did-for-milk/

Its proprietary fermentation process will enable QOA to scale production by 2035 and be able to price its "chocolate" products the same or below the cost of traditional chocolate.

Links:
[1]: https://techcrunch.com/2021/10/27/qoa-brings-in-seed-round-to-do-for-chocolate-what-oatly-did-for-milk/ (link to the article)
```

**input:**
 ```sh
 rss-reader  https://techcrunch.com/feed/  --limit 1 --to-html ~/Downloads

```

**output:**
 ```sh
Feed: TechCrunch

Title: QOA brings in seed round to do for chocolate what Oatly did for milk

Date: Wed, 27 Oct 2021 10:00:15 +0000
Link: https://techcrunch.com/2021/10/27/qoa-brings-in-seed-round-to-do-for-chocolate-what-oatly-did-for-milk/

Its proprietary fermentation process will enable QOA to scale production by 2035 and be able to price its "chocolate" products the same or below the cost of traditional chocolate.

Links:
[1]: https://techcrunch.com/2021/10/27/qoa-brings-in-seed-round-to-do-for-chocolate-what-oatly-did-for-milk/ (link to the article)

```


![html_file_screen](https://res.cloudinary.com/dilcm4lrv/image/upload/v1635332191/test_gfz7as.jpg)







