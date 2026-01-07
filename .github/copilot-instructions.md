import requests
from requests.auth import HTTPBasicAuth


HATENA_ID = "fromkorea"
BLOG_ID = "fromkorea.hatenablog.com"
API_KEY = "v7xihn2bfk"


url = f"https://blog.hatena.ne.jp/fromkorea/fromkorea.hatenablog.com/atom/entry"


xml = f"""
<entry xmlns="http://www.w3.org/2005/Atom">
<title>APIテスト投稿</title>
<content type="text/plain">これはAPIからの自動投稿です。</content>
</entry>
"""


res = requests.post(
url,
data=xml.encode("utf-8"),
headers={"Content-Type": "application/atom+xml"},
auth=HTTPBasicAuth(HATENA_ID, API_KEY)
)


print(res.status_code)