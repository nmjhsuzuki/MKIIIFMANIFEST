# [MKIIIFMANIFEST](https://github.com/nmjhsuzuki/MKIIIFMANIFEST)

# 特徴 (Features)

　[MKIIIFMANIFEST](https://github.com/nmjhsuzuki/MKIIIFMANIFEST)は，[IIIF Presentation API](https://iiif.io/api/presentation/2.1/) に準拠するほぼ最小の IIIF manifest を生成します．Python3 で書かれています．

　[MKIIIFMANIFEST](https://github.com/nmjhsuzuki/MKIIIFMANIFEST) is a tiny IIIF manifest generator. It is written in Python3.

# デモンストレーション (Demonstration)

　MKIIIFMANIFEST によって IIIF manifest を作れるのは，IIIF image API で画像情報リクエストできる画像が対象となります．  
　ここでは [http://nmjhsuztak.xii.jp/cgi-bin/dzi-iiif.cgi?IIIF=/Newcastle/info.json](http://nmjhsuztak.xii.jp/cgi-bin/dzi-iiif.cgi?IIIF=/Newcastle/info.json)を対象とします．MKIIIFMANIFEST の入力となる data.json は以下のようになります．  

```JSON
{
        "Newcastle": {
                "imagebaseURI": "http://nmjhsuztak.xii.jp/cgi-bin/dzi-iiif.cgi?IIIF=",
                "manifestbaseURI": "http://nmjhsuztak.xii.jp/IIIF",
                "label": "City of Newcastle upon Tyne",
                "description": "Snapshots of City of Newcaslte upon Type in July 2013.",
                "license": "https://creativecommons.org/publicdomain/zero/1.0/",
                "attribution": "CC0 1.0 Universal (CC0 1.0) Public Domain Dedication"
        }
}
```

　"Newcastle" が識別子(identifier)を表します．  
　imagebaseURI には，マニフェストが指すIIIF画像の基底URIを与えます．  
　manifestbaseURI には，作成したマニフェストが置かれるディレクトリのURIを与えます．この場合はマニフェストが http://nmjhsuzuki.xii.jp/IIIF/ に Newcastle.json という名前で置かれることを指示しています．  
　imagebaseURI と manifestbaseURI は省略すると，スクリプト内で定義されたデフォルト値が使われます．  
　label, description, license, attribution の４つのタグをマニフェスト内に書き込めます．それぞれの意味は[IIIF presentation API](https://iiif.io/api/presentation/2.1/)を参照してください．  
　label を省略すると，識別子(この場合は "Newcastle"）が使われます．description, license, attribution は省略できます．  

　data.json に複数のマニフェスト定義情報を与えて，一度に複数のマニフェストを作ることもできます：

```JSON
{
        "Edozu": { ... },
        "RakuchuA": { ... },
        ...
        "RakuchuF": { ... }
}
```

　これから MKIIIFMANIFEST によって作成したファイルが以下の output\Newcastle.json です．ただ１枚の画像からなる IIIF コンテンツを表しています．  

```JSON
{
    "@context": "http://iiif.io/api/presentation/2/context.json",
    "@id": "http://nmjhsuzuki.xii.jp/IIIF/Newcastle.json",
    "@type": "sc:Manifest",
    "label": "City of Newcastle upon Tyne",
    "description": "Snapshots of City of Newcaslte upon Type in July 2013.",
    "license": "https://creativecommons.org/publicdomain/zero/1.0/",
    "attribution": "CC0 1.0 Universal (CC0 1.0) Public Domain Dedication",
    "thumbnail": {
        "@id": "http://nmjhsuzuki.xii.jp/cgi-bin/dzi-iiif.cgi?IIIF=/Newcastle/full/400,/0/default.jpg"
    },
    "sequences": [
        {
            "@type": "sc:Sequence",
            "canvases": [
                {
                    "@type": "sc:Canvas",
                    "@id": "http://nmjhsuzuki.xii.jp/IIIF/Newcastle/canvas",
                    "width": 21600,
                    "height": 10800,
                    "images": [
                        {
                            "@type": "oa:Annotation",
                            "motivation": "sc:painting",
                            "resource": {
                                "@id": "http://nmjhsuzuki.xii.jp/cgi-bin/dzi-iiif.cgi?IIIF=/Newcastle/full/full/0/default.jpg",
                                "@type": "dctypes:Image",
                                "service": {
                                    "@context": "http://iiif.io/api/image/2/context.json",
                                    "@id": "http://nmjhsuzuki.xii.jp/cgi-bin/dzi-iiif.cgi?IIIF=/Newcastle",
                                    "profile": "http://iiif.io/api/image/2/level1.json"
                                }
                            },
                            "on": "http://nmjhsuzuki.xii.jp/IIIF/Newcastle/canvas"
                        }
                    ]
                }
            ]
        }
    ]
}
```

　IIIF manifest があると [mirador](https://projectmirador.org/) や [IIIF Curation Viewer](http://codh.rois.ac.jp/software/iiif-curation-viewer/) などの IIIF ビューワで画像を見ることができます．IIIF Curation Viewer による表示例: [http://nmjhsuzuki.xii.jp/ICViewer/index.html?manifest=http://nmjhsuzuki.xii.jp/IIIF/Newcastle.json](http://nmjhsuzuki.xii.jp/ICViewer/index.html?manifest=http://nmjhsuzuki.xii.jp/IIIF/Newcastle.json)．

# 背景 (Background)

　[IIIF (International Image Interoperability Framework)](https://iiif.io) は，画像へのアクセスを標準化し相互運用性を確保するための国際的なコミュニティ活動です([Wikipedia](https://ja.wikipedia.org/wiki/International_Image_Interoperability_Framework)より)．[IIIF Image API](https://iiif.io/api/image/2.1/)は，画像の任意の部分に任意の大きさでアクセスするための統一的なインターフェイスを定義しており，[IIIF presentation API](https://iiif.io/api/presentation/2.1/) は，構造を持つ IIIF コンテンツの記述や，ライセンス等の画像に関する諸情報を記述する統一的な手段を与えています．  
　今回，[DZI-IIIF](https://github.com/nmjhsuzuki/DZI-IIIF)の公開に合わせて，作成した IIIF 画像を IIIF ビューワでただちに確認できるよう，このスクリプトを作成しました．  

# 必要な環境 (Requirement)

　Python3 が必要です．

　Windows10 Professional 64bit（バージョン1909）上で，以下の環境でテストしています．

* Python 3.8.3 (64bit版)

　さくらのレンタルサーバースタンダード(FreeBSD 9.1-RELEASE-p24 amd64)上で，以下の環境でテストしています．

* Python 3.5.9

# インストール(Installation)

　任意のディレクトリに git clone してご利用ください．  
　スクリプト内の設定情報や入力ファイル(data.json)は，Windows10 上に IIS をインストールを導入してローカルでテストすることを想定した設定になっています．すなわち，生成されたマニフェストは C:\inetpub\wwwroot\IIIF に Newcastle.json という名前でおかれ，Webブラウザからは http://localhost/IIIF/Newcastle.json というURIでアクセスすることを想定しています．

```Python
...
# --- 利用者環境に応じて設定する情報 ここから ---

# 入力: json ファイル (utf-8)
# {<Identifier>: { "imagebaseURI": ..., "manifestbaseURI": ..., "label": ..., "description":, "license": ..., "attribution": }, ...}
indata_path = os.path.join(os.getcwd(), 'data.json')

# 出力: json ファイル (utf-8)
# manifest は <identifier>.json という名前で作られる．
# 出力ディレクトリ
outdata_dir_path = os.path.join(os.getcwd(), 'output')
    
# info.json を問い合わせる IIIF サーバ のURIのデフォルト値
default_iiif_server_uri_header = 'http://localhost/cgi-bin/dzi-iiif.cgi?IIIF='

# manifest が置かれるディレクトリのURIのデフォルト値
default_manifest_uri_header = 'http://localhost/IIIF'

# サムネイル画像の大きさ（横幅）
thumbnail_width = 400

# --- 利用者環境に応じて設定する情報 ここまで ---
...
```
```JSON
{
        "Newcastle": {
                "label": "City of Newcastle upon Tyne",
                "description": "Snapshots of City of Newcaslte upon Type in July 2013.",
                "license": "https://creativecommons.org/publicdomain/zero/1.0/",
                "attribution": "CC0 1.0 Universal (CC0 1.0) Public Domain Dedication"
        }
}
```

　スクリプトを実行すると output\ 以下に Newcastle.json が作られます．

```Batchfile
D:\MKIIIFMANIFEST>python mkIIIFmanifest.py
...
D:\MKIIIFMANIFEST>copy output\Newcastle.json C:\inetpub\wwwroot\IIIF
```

# その他 (Note)

　最初のバージョンを 2020年6月1日に公開しました．  

# 作者情報 (Author)

* 鈴木卓治 (SUZUKI, Takuzi)
* 国立歴史民俗博物館 (National Museum of Japanese History, Chiba, JAPAN)
* Email: suzuki@rekihaku.ac.jp
* Twitter: @digirekiten （デジタルで楽しむ歴史資料 https://twitter.com/digirekiten ）

# ライセンス (License)

　"MKIIIFMANIFEST"は[MIT license](https://en.wikipedia.org/wiki/MIT_License)に従います．

　"MKIIIFMANIFEST" is under [MIT license](https://en.wikipedia.org/wiki/MIT_License).

