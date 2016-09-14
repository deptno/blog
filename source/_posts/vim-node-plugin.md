---
title: vim-node-plugin
tags: 'vim, neovim, vim plugin node, vim plugin javascript'
date: 2016-08-03 00:51:41
---


VIM 플러그인을 node로 작성하는 방법에 대해 간단히 리포트한다. 기본적으로 지원하는 플러그인은 vimscript, python정도로 보이는데 작은 기능을 추가하려고하는데 언어까지 배우기는 뭐해서 방법을 찾아봤다. 플러그인의 기능을까지라고 할 수 있을진 모르겠지만 node를 통해 간단한 작업은 할 수 있다.

OSX를 기준으로 작성되었고 [neovim](http://bglee.me/posts/2016/nvim/)을 사용하고 있다. 홈 디렉토리 안에 `.vimrc`파일이 설정일텐데 `neovim`에서는 `.config/nvim/init.vim`파일이 그 역할을 대신한다. 여튼 설정 파일을 열고 다음과 같이 추가해준다.

코드가 우아하진 않고 공유차원이 그러려니...

```vim
function CallNode(...) 
    execute '%! node -e "require(\"$HOME/.config/node-connector\")[\"' . a:1 . '\"][\"' . a:2 . '\"]()"'
endfunction

function CallNodeWithEcho(...) 
    echom system('node -e "require(\"$HOME/.config/node-connector\")[\"' . a:1 . '\"][\"' . a:2 . '\"](\"' . a:3 . '\")"')
endfunction

nmap ,fj :<C-U>call CallNode("format", "json")<CR>
nmap ,fx :<C-U>call CallNode("format", "xml")<CR>
nmap ,trk :<C-U>call CallNodeWithEcho("translate", "en", getline("."))<CR>
nmap ,tre :<C-U>call CallNodeWithEcho("translate", "ko", getline("."))<CR>
nmap ,trj :<C-U>call CallNodeWithEcho("translate", "ja", getline("."))<CR>
```

`CallNode` 와 `CallNodeWithEcho` 함수가 있는데 후자는 결과 값을 VIM의 status bar에 표시해주는 역할을 한다.

아래 다섯개의 함수를 정의했는데 역할은 순서대로 다음과 같다.

* formating json
* formating xml
* 영어로 번역
* 한국어로 번역
* 일본어로 번역

함수의 정의를 보면 알겠지만 단순히 node 스크립트를 로딩해서 실행하는 것이다.

`$HOME/.config/node-connector` node script를 로드하므로 이 파일을 작성해줘야한다.

`node-connector.js`

```js
var pd = require('pretty-data').pd;
module.exports = {
    format: {
        json: function() {
            let i = process.stdin, d = '';
            i.resume();
            i.setEncoding('utf8');
            i.on('data', function(data) { d += data; });
            i.on('end', function() {
                try {
                    console.log(JSON.stringify(JSON.parse(d), null, 4));
                } catch(ex) {
                    console.log(d);
                }
            });
        },
        xml: function() {
            let i = process.stdin, d = '';
            i.resume();
            i.setEncoding('utf8');
            i.on('data', function(data) { d += data; });
            i.on('end', function() {
                try {
                    console.log(pd.xml(d));
                } catch(ex) {
                    console.log(d);
                }
            });
        }
    },
    translate: {
        en: function(text) {
            this.tr('en', text);
        },
        ja: function(text) {
            this.tr('ja', text);
        },
        ko: function(text) {
            this.tr('ko', text);
        },
        tr: function(source, text) {
            var https = require('https');

            var client_id = ''; //TODO: use yours
            var client_secret = ''; //TODO: use yours
            var host = 'openapi.naver.com';
            var port = 443;
            var uri = '/v1/language/translate';

            var data = require('querystring').stringify({
                source: source,
                target: source !== 'ko' ? 'ko' : 'en',
                text: text
            });

            var options = {
                host: host,
                port: port,
                path: uri,
                method: 'POST',
                headers: {
                    'X-Naver-Client-Id':client_id,
                    'X-Naver-Client-Secret': client_secret,
                    'Content-Type': 'application/x-www-form-urlencoded',
                    'Content-Length': Buffer.byteLength(data)
                }
            };

            var req = https.request(options, function(res) {
                res.setEncoding('utf8');
                res.on('data', function (chunk) {
                    console.log(JSON.parse(chunk).message.result.translatedText);
                });
            });
            req.write(data);
            req.end();
        }
    }
};
```

번역 api를 서비스로 네이버 개발자용을 쓰고 있어서 이 부분은 각자 발급해서 본인 것을 쓰면 된다.

이제 문장이 있는 줄에서 영어로 번역을 원한다면 `,trk`를 누르면 된다. json파일을 포맷팅하고 싶다면 `,fj` 이런식이다. 오류가 발생하면 그대로 원본글 그대로를 보여주게 해놓았다.

![](https://lh3.googleusercontent.com/Fe-tFQizH0CoxQ9FhCGlr5eSMqytEXRVb3WUjeADnxQy_IHY2RDovBceHDmGe3vEFkd0FART-ZiznSvQyyVOjxVXqEtdfMfL7oMD2IKGUHFylzebdqfgox9NKNuufM6WB5kE14MKYCOgIkCTQCWsyGP5lxSXShg0ecsO-D9AHT_ZTQpMjtlAZkxSuXucykq_WAFuiicghMsDFSpTwKmWSt0DQIXJxX6ZpzMmV8jOD_wKwg0kxswsmeYBTAyL51eGAOXzQSHdsJziq6bu3B-8qcEII9UW-isg09QzMMzQVnxiFw1NqsDZQyPryNaZFV5PRQqU-CQxXFchsmQ9KDmTcc1jf2k0tt7DeSXUMaZSILpkPvTd4x6HoTzCM7_ZVQYxo1J3Hz1BhrtzgosJ9imjPbS-B8fz4cpXoNh_UqH_D2d-iEDA1aVg3BiuvgHJqsmHKrXUgt5VEinQri3CJSfctutGGXNB81X1V-e8WF3GM5u2ZsqB6MnAU8RAstE72Q_DqmsEcVqcY_EnZnP1tOu93vdqrByjrdzcnr8snG3tEYM_ozDnhLVesTDiYduND_G6ynoLUHt-AlrjEA3nRsrB0F8LHfsP6oms_QfrjgjT04CBOQ09UA=w191-h176-no)

더 참조가 필요하다면 필자의 셋업을 참조하도록한다. 보통 난 노트북을 새로 셋업할때 git을 깔고 처음으로 설정을 클론뜨도록 해두었다. vim은 터미널 환경의 반이니까.

[https://github.com/deptno/.config](https://github.com/deptno/.config)
