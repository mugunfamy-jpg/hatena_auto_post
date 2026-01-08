# hatena_auto_post
import json
import requests
from requests.auth import HTTPBasicAuth

def post_entry(config_path="config.json"):
    # 設定ファイル読み込み
    with open(config_path, "r", encoding="utf-8") as f:
        config = json.load(f)

    blog_id = config["blog_id"]
    username = config["username"]
    api_key = config["api_key"]

    # AtomPub API エンドポイント
    url = f"https://blog.hatena.ne.jp/{username}/{blog_id}/atom/entry"

    # XML形式で記事を作成
    entry_xml = f"""<?xml version="1.0" encoding="utf-8"?>
    <entry xmlns="http://www.w3.org/2005/Atom"
           xmlns:app="http://www.w3.org/2007/app">
      <title>{config['title']}</title>
      <content type="text/plain">{config['content']}</content>
      {''.join([f'<category term="{c}" />' for c in config['categories']])}
      <app:control>
        <app:draft>{"yes" if config['draft'] else "no"}</app:draft>
      </app:control>
    </entry>
    """

    # 投稿リクエスト
    response = requests.post(
        url,
        data=entry_xml.encode("utf-8"),
        headers={"Content-Type": "application/atom+xml"},
        auth=HTTPBasicAuth(username, api_key)
    )

    if response.status_code == 201:
        print("✅ 投稿成功!")
    else:
        print("❌ 投稿失敗:", response.status_code, response.text)

if __name__ == "__main__":
    post_entry()