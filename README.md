# api documentation for  [keyword-extractor (v0.0.13)](https://github.com/michaeldelorenzo/keyword-extractor)  [![npm package](https://img.shields.io/npm/v/npmdoc-keyword-extractor.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-keyword-extractor) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-keyword-extractor.svg)](https://travis-ci.org/npmdoc/node-npmdoc-keyword-extractor)
#### Module for creating a keyword array from a string and excluding stop words.

[![NPM](https://nodei.co/npm/keyword-extractor.png?downloads=true)](https://www.npmjs.com/package/keyword-extractor)

[![apidoc](https://npmdoc.github.io/node-npmdoc-keyword-extractor/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-keyword-extractor_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-keyword-extractor/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-keyword-extractor/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-keyword-extractor/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Michael De Lorenzo",
        "email": "michael@delorenzodesign.com",
        "url": "http://mikedelorenzo.com"
    },
    "bugs": {
        "url": "https://github.com/michaeldelorenzo/keyword-extractor/issues"
    },
    "contributors": [
        {
            "name": "Marcos Sanz",
            "email": "marcossanzlatorre@gmail.com",
            "url": "http://www.mistersanz.com"
        },
        {
            "name": "d-oliveros",
            "url": "https://github.com/d-oliveros"
        },
        {
            "name": "Abhijeet Sutar",
            "email": "ajduke@about.me",
            "url": "https://github.com/ajduke"
        },
        {
            "name": "janvde",
            "url": "https://github.com/janvde"
        },
        {
            "name": "Alex Gustafsson",
            "url": "https://github.com/AlexGustafsson"
        }
    ],
    "dependencies": {
        "underscore": "1.7.0",
        "underscore.string": "2.3.3"
    },
    "description": "Module for creating a keyword array from a string and excluding stop words.",
    "devDependencies": {
        "mocha": "*",
        "should": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "07bad59d3a4344111c40ffface42da34a949e419",
        "tarball": "https://registry.npmjs.org/keyword-extractor/-/keyword-extractor-0.0.13.tgz"
    },
    "engines": {
        "node": "node >= 0.10.0"
    },
    "gitHead": "4c486d060ecf9837b824a8a7b1348259034b9c28",
    "homepage": "https://github.com/michaeldelorenzo/keyword-extractor",
    "keywords": [
        "text",
        "keyword",
        "search",
        "extract"
    ],
    "licenses": [
        {
            "type": "MIT",
            "url": "https://github.com/michaeldelorenzo/keyword-extractor/raw/master/LICENSE"
        }
    ],
    "main": "./index",
    "maintainers": [
        {
            "name": "mikedelorenzo",
            "email": "michael@delorenzodesign.com"
        }
    ],
    "name": "keyword-extractor",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/michaeldelorenzo/keyword-extractor.git"
    },
    "scripts": {},
    "version": "0.0.13",
    "warnings": [
        {
            "code": "ENOTSUP",
            "required": {
                "node": "node >= 0.10.0"
            },
            "pkgid": "keyword-extractor@0.0.13"
        },
        {
            "code": "ENOTSUP",
            "required": {
                "node": "node >= 0.10.0"
            },
            "pkgid": "keyword-extractor@0.0.13"
        }
    ]
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module keyword-extractor](#apidoc.module.keyword-extractor)
1.  [function <span class="apidocSignatureSpan">keyword-extractor.</span>extract (str, options)](#apidoc.element.keyword-extractor.extract)
1.  [function <span class="apidocSignatureSpan">keyword-extractor.</span>getStopwords (options)](#apidoc.element.keyword-extractor.getStopwords)



# <a name="apidoc.module.keyword-extractor"></a>[module keyword-extractor](#apidoc.module.keyword-extractor)

#### <a name="apidoc.element.keyword-extractor.extract"></a>[function <span class="apidocSignatureSpan">keyword-extractor.</span>extract (str, options)](#apidoc.element.keyword-extractor.extract)
- description and source-code
```javascript
function _extract(str, options){
    if(_.isEmpty(str)){
        return [];
    }
    if(_.isEmpty(options)){
        options = {
            remove_digits: true,
            return_changed_case: true
        };
    }
    var return_changed_case = options.return_changed_case;
    var return_chained_words = options.return_chained_words;
    var remove_digits = options.remove_digits;
    var _language = options.language || "english";
    var _remove_duplicates = options.remove_duplicates || false;

    if(supported_languages.indexOf(_language) < 0){
        throw new Error("Language must be one of ["+supported_languages.join(",")+"]");
    }

    //  strip any HTML and trim whitespace
    var text = _.str.trim(_.str.stripTags(str));
    if(_.isEmpty(text)){
        return [];
    }else{
        var words = text.split(/\s/);
        var unchanged_words = [];
        var low_words = [];
        //  change the case of all the words
        for(var x = 0;x < words.length; x++){
            var w = words[x].match(/https?:\/\/.*[\r\n]*/g) ? words[x] : words[x].replace(/\.|,|;|!|\?|\(|\)|:|"|^'|'$/g,'');
            //  remove periods, question marks, exclamation points, commas, and semi-colons
            //  if this is a short result, make sure it's not a single character or something 'odd'
            if(w.length === 1){
                w = w.replace(/-|_|@|&|#/g,'');
            }
            //  if it's a number, remove it
            var digits_match = w.match(/\d/g);
            if(remove_digits && digits_match && digits_match.length === w.length){
                w = "";
            }
            if(w.length > 0){
                low_words.push(w.toLowerCase());
                unchanged_words.push(w);
            }
        }
        var results = [];
        var _stopwords = options.stopwords || _getStopwords({ language: _language });
        var _last_result_word_index = 0;
        for(var y = 0; y < low_words.length; y++){

            if(_stopwords.indexOf(low_words[y]) < 0){
                var result_word = return_changed_case && !unchanged_words[y].match(/https?:\/\/.*[\r\n]*/g) ? low_words[y] : unchanged_words
[y];

                if (return_chained_words && _last_result_word_index === y - 1) {
                  var change_pos = results.length - 1 < 0 ? 0 : results.length - 1;
                  results[change_pos] = results[change_pos] ? results[change_pos] + ' ' + result_word : result_word;
                } else {
                  results.push(result_word);
                }

                _last_result_word_index = y;
            }
        }

        if(_remove_duplicates) {
            results= _.uniq(results, function (item) {
                return item;
            });
        }

        return results;
    }
}
```
- example usage
```shell
...
var keyword_extractor = require("keyword-extractor");

//  Opening sentence to NY Times Article at
//  http://www.nytimes.com/2013/09/10/world/middleeast/surprise-russian-proposal-catches-obama-between-putin-and-house-republicans
.html
var sentence = "President Obama woke up Monday facing a Congressional defeat that many in both parties believed could hobble his
 presidency."

//  Extract the keywords
var extraction_result = keyword_extractor.extract(sentence,{
     language:"english",
     remove_digits: true,
     return_changed_case:true,
     remove_duplicates: false

});
...
```

#### <a name="apidoc.element.keyword-extractor.getStopwords"></a>[function <span class="apidocSignatureSpan">keyword-extractor.</span>getStopwords (options)](#apidoc.element.keyword-extractor.getStopwords)
- description and source-code
```javascript
function _getStopwords(options){
    options = options || {};

    var _language = options.language || "english";
    if(supported_languages.indexOf(_language) < 0){
        throw new Error("Language must be one of ["+supported_languages.join(",")+"]");
    }

    return stopwords[_language];
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
