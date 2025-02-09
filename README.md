## ruia-pyppeteer

A [Ruia](https://github.com/howie6879/ruia) plugin for loading javascript

> Notice:  Works on ruia >= 0.4.9

### Installation

```shell
pip install ruia_pyppeteer
# New features
pip install git+https://github.com/ruia-plugins/ruia-pyppeteer
```

### Usage

`ruia_pyppeteer` will load js by using pyppeteer.
 
 You need to pay attention when you use load_js, it will download a recent version of Chromium (~100MB). This only happens once.

**Load JavaScript**

```python
import asyncio

from ruia_pyppeteer import PyppeteerRequest as Request

request = Request("https://www.jianshu.com/", load_js=True)
response = asyncio.get_event_loop().run_until_complete(request.fetch())
print(response)
```

**Complete example**

```python
from ruia import AttrField, TextField, Item

from ruia_pyppeteer import PyppeteerSpider as Spider


class JianshuItem(Item):
    target_item = TextField(css_select='ul.list>li')
    author_name = TextField(css_select='a.name')
    author_url = AttrField(attr='href', css_select='a.name')

    async def clean_author_url(self, author_url):
        return f"https://www.jianshu.com{author_url}"


class JianshuSpider(Spider):
    start_urls = ['https://www.jianshu.com/']
    concurrency = 10

    async def parse(self, response):
        async for item in JianshuItem.get_items(html=response.html):
            # Loading js by using PyppeteerRequest
            print(item)


if __name__ == '__main__':
    JianshuSpider.start()
```

Enjoy it :)
