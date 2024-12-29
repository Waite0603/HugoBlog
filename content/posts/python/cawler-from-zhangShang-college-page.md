---
title: "某高考平台逆向"
date: 2023-09-22T16:04:23+08:00
categories: ["Python"]
tags: ["Cawler"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---


> 以下代码取的是院校专业分数线

![_20221027212744](https://qiniu.waite.wang/202309151459082.png)

> F12 得到接口  
> https://api.eol.cn/web/api/?local\_batch\_id=14&local\_province\_id=31&local\_type\_id=3&page=1&school\_id=31&size=10&special\_group=&uri=apidata/api/gk/score/special&year=2021&signsafe=6cdbc334a395abd2a99b9bd8cc29c42f  
> ![_20221027213021](https://qiniu.waite.wang/202309151459515.png)
> 
> 推断参数含义 
> ![_20221027213205](https://qiniu.waite.wang/202309151459905.png)

### signsafe 数据加密签名获取

> 搜索 锁定 signsafe 字段以及加密

![image](https://qiniu.waite.wang/202309151459897.png)

以上看到，t就是请求中的signsafe参数。而且，函数中也出现了HmacSHA1、base64等方法。由此可知，定位的加密位置是正确的。接下来需要将加密函数抠下来改写，方便调用。下断点继续跟踪方法调用。跟踪v.a.enc.Utf8.parse方法，如下多次尝试, 最终试出

```
var CryptoJS = require("crypto-js");
function signsafe_jami(p) {
    p = p.replace(/^\/|https?:\/\/\/?/, "");
    p = p.replace(/%2F/g, "/");

    console.log(p)
    let f = CryptoJS.HmacSHA1(CryptoJS.enc.Utf8.parse(p), "D23ABC@#56");

    f = CryptoJS.enc.Base64.stringify(f).toString();

    f = CryptoJS.MD5(f).toString();
    return f;
}

url = "https://api.eol.cn/web/api/?local_batch_id=14&local_province_id=31&local_type_id=3&page=1&school_id=31&size=100&special_group=&uri=apidata%2Fapi%2Fgk%2Fscore%2Fspecial&year=2021"
console.log(signsafe_jami(url))
```

> 开始前 先安装 npm install crypto-js

### data 解析

> 通过以上 signsafe 访问文章开头接口, 拿到一长串 data 数据(见开头图2)

从 method 知道使用的是 aes-256-cbc的方式进行加密, 推出加密方法是aes-256， 密钥长度256位，cbc指的是在加密和解密是需要一个初始化向量(Initialization Vector, IV)，在每次加密之前或者解密之后，使用初始化向量与明文或密文异或。说白了，就是设定的密钥，添加一个初始化偏移量。加密时，明文先与iv偏移量异或，再将结果进行256位的块加密，得到的输出就是密文，同时本次的输出密文作为下一个块加密的IV。

> 通过搜索 aes 确定疑似代码 进行仿写

![imagea8d0a96f1aefbdac](https://qiniu.waite.wang/202309151500499.png)

![image48b364e106ed8429.png](images/image48b364e106ed8429.png)

> 以下代码参考

```
var CryptoJS = CryptoJS || (function (Math, undefined) {
    var C = {};
    var C_lib = C.lib = {};
    var Base = C_lib.Base = (function () {
        function F() {};
        return {
            extend: function (overrides) {
                F.prototype = this;
                var subtype = new F();
                if (overrides) {
                    subtype.mixIn(overrides);
                }
                if (!subtype.hasOwnProperty('init') || this.init === subtype.init) {
                    subtype.init = function () {
                        subtype.$super.init.apply(this, arguments);
                    };
                }
                subtype.init.prototype = subtype;
                subtype.$super = this;
                return subtype;
            }, create: function () {
                var instance = this.extend();
                instance.init.apply(instance, arguments);
                return instance;
            }, init: function () {}, mixIn: function (properties) {
                for (var propertyName in properties) {
                    if (properties.hasOwnProperty(propertyName)) {
                        this[propertyName] = properties[propertyName];
                    }
                }
                if (properties.hasOwnProperty('toString')) {
                    this.toString = properties.toString;
                }
            }, clone: function () {
                return this.init.prototype.extend(this);
            }
        };
    }());
    var WordArray = C_lib.WordArray = Base.extend({
        init: function (words, sigBytes) {
            words = this.words = words || [];
            if (sigBytes != undefined) {
                this.sigBytes = sigBytes;
            } else {
                this.sigBytes = words.length * 4;
            }
        }, toString: function (encoder) {
            return (encoder || Hex).stringify(this);
        }, concat: function (wordArray) {
            var thisWords = this.words;
            var thatWords = wordArray.words;
            var thisSigBytes = this.sigBytes;
            var thatSigBytes = wordArray.sigBytes;
            this.clamp();
            if (thisSigBytes % 4) {
                for (var i = 0; i < thatSigBytes; i++) {
                    var thatByte = (thatWords[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                    thisWords[(thisSigBytes + i) >>> 2] |= thatByte << (24 - ((thisSigBytes + i) % 4) * 8);
                }
            } else if (thatWords.length > 0xffff) {
                for (var i = 0; i < thatSigBytes; i += 4) {
                    thisWords[(thisSigBytes + i) >>> 2] = thatWords[i >>> 2];
                }
            } else {
                thisWords.push.apply(thisWords, thatWords);
            }
            this.sigBytes += thatSigBytes;
            return this;
        }, clamp: function () {
            var words = this.words;
            var sigBytes = this.sigBytes;
            words[sigBytes >>> 2] &= 0xffffffff << (32 - (sigBytes % 4) * 8);
            words.length = Math.ceil(sigBytes / 4);
        }, clone: function () {
            var clone = Base.clone.call(this);
            clone.words = this.words.slice(0);
            return clone;
        }, random: function (nBytes) {
            var words = [];
            var r = (function (m_w) {
                var m_w = m_w;
                var m_z = 0x3ade68b1;
                var mask = 0xffffffff;
                return function () {
                    m_z = (0x9069 * (m_z & 0xFFFF) + (m_z >> 0x10)) & mask;
                    m_w = (0x4650 * (m_w & 0xFFFF) + (m_w >> 0x10)) & mask;
                    var result = ((m_z << 0x10) + m_w) & mask;
                    result /= 0x100000000;
                    result += 0.5;
                    return result * (Math.random() > .5 ? 1 : -1);
                }
            });
            for (var i = 0, rcache; i < nBytes; i += 4) {
                var _r = r((rcache || Math.random()) * 0x100000000);
                rcache = _r() * 0x3ade67b7;
                words.push((_r() * 0x100000000) | 0);
            }
            return new WordArray.init(words, nBytes);
        }
    });
    var C_enc = C.enc = {};
    var Hex = C_enc.Hex = {
        stringify: function (wordArray) {
            var words = wordArray.words;
            var sigBytes = wordArray.sigBytes;
            var hexChars = [];
            for (var i = 0; i < sigBytes; i++) {
                var bite = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                hexChars.push((bite >>> 4).toString(16));
                hexChars.push((bite & 0x0f).toString(16));
            }
            return hexChars.join('');
        }, parse: function (hexStr) {
            var hexStrLength = hexStr.length;
            var words = [];
            for (var i = 0; i < hexStrLength; i += 2) {
                words[i >>> 3] |= parseInt(hexStr.substr(i, 2), 16) << (24 - (i % 8) * 4);
            }
            return new WordArray.init(words, hexStrLength / 2);
        }
    };
    var Latin1 = C_enc.Latin1 = {
        stringify: function (wordArray) {
            var words = wordArray.words;
            var sigBytes = wordArray.sigBytes;
            var latin1Chars = [];
            for (var i = 0; i < sigBytes; i++) {
                var bite = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                latin1Chars.push(String.fromCharCode(bite));
            }
            return latin1Chars.join('');
        }, parse: function (latin1Str) {
            var latin1StrLength = latin1Str.length;
            var words = [];
            for (var i = 0; i < latin1StrLength; i++) {
                words[i >>> 2] |= (latin1Str.charCodeAt(i) & 0xff) << (24 - (i % 4) * 8);
            }
            return new WordArray.init(words, latin1StrLength);
        }
    };
    var Utf8 = C_enc.Utf8 = {
        stringify: function (wordArray) {
            try {
                return decodeURIComponent(escape(Latin1.stringify(wordArray)));
            } catch (e) {
                throw new Error('Malformed UTF-8 data');
            }
        }, parse: function (utf8Str) {
            return Latin1.parse(unescape(encodeURIComponent(utf8Str)));
        }
    };
    var BufferedBlockAlgorithm = C_lib.BufferedBlockAlgorithm = Base.extend({
        reset: function () {
            this._data = new WordArray.init();
            this._nDataBytes = 0;
        }, _append: function (data) {
            if (typeof data == 'string') {
                data = Utf8.parse(data);
            }
            this._data.concat(data);
            this._nDataBytes += data.sigBytes;
        }, _process: function (doFlush) {
            var data = this._data;
            var dataWords = data.words;
            var dataSigBytes = data.sigBytes;
            var blockSize = this.blockSize;
            var blockSizeBytes = blockSize * 4;
            var nBlocksReady = dataSigBytes / blockSizeBytes;
            if (doFlush) {
                nBlocksReady = Math.ceil(nBlocksReady);
            } else {
                nBlocksReady = Math.max((nBlocksReady | 0) - this._minBufferSize, 0);
            }
            var nWordsReady = nBlocksReady * blockSize;
            var nBytesReady = Math.min(nWordsReady * 4, dataSigBytes);
            if (nWordsReady) {
                for (var offset = 0; offset < nWordsReady; offset += blockSize) {
                    this._doProcessBlock(dataWords, offset);
                }
                var processedWords = dataWords.splice(0, nWordsReady);
                data.sigBytes -= nBytesReady;
            }
            return new WordArray.init(processedWords, nBytesReady);
        }, clone: function () {
            var clone = Base.clone.call(this);
            clone._data = this._data.clone();
            return clone;
        }, _minBufferSize: 0
    });
    var Hasher = C_lib.Hasher = BufferedBlockAlgorithm.extend({
        cfg: Base.extend(),
        init: function (cfg) {
            this.cfg = this.cfg.extend(cfg);
            this.reset();
        }, reset: function () {
            BufferedBlockAlgorithm.reset.call(this);
            this._doReset();
        }, update: function (messageUpdate) {
            this._append(messageUpdate);
            this._process();
            return this;
        }, finalize: function (messageUpdate) {
            if (messageUpdate) {
                this._append(messageUpdate);
            }
            var hash = this._doFinalize();
            return hash;
        }, blockSize: 512 / 32,
        _createHelper: function (hasher) {
            return function (message, cfg) {
                return new hasher.init(cfg).finalize(message);
            };
        }, _createHmacHelper: function (hasher) {
            return function (message, key) {
                return new C_algo.HMAC.init(hasher, key).finalize(message);
            };
        }
    });
    var C_algo = C.algo = {};
    return C;
}(Math));

(function () {
    var C = CryptoJS;
    var C_lib = C.lib;
    var WordArray = C_lib.WordArray;
    var C_enc = C.enc;
    var Base64 = C_enc.Base64 = {
        stringify: function (wordArray) {
            var words = wordArray.words;
            var sigBytes = wordArray.sigBytes;
            var map = this._map;
            wordArray.clamp();
            var base64Chars = [];
            for (var i = 0; i < sigBytes; i += 3) {
                var byte1 = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                var byte2 = (words[(i + 1) >>> 2] >>> (24 - ((i + 1) % 4) * 8)) & 0xff;
                var byte3 = (words[(i + 2) >>> 2] >>> (24 - ((i + 2) % 4) * 8)) & 0xff;
                var triplet = (byte1 << 16) | (byte2 << 8) | byte3;
                for (var j = 0;
                    (j < 4) && (i + j * 0.75 < sigBytes); j++) {
                    base64Chars.push(map.charAt((triplet >>> (6 * (3 - j))) & 0x3f));
                }
            }
            var paddingChar = map.charAt(64);
            if (paddingChar) {
                while (base64Chars.length % 4) {
                    base64Chars.push(paddingChar);
                }
            }
            return base64Chars.join('');
        }, parse: function (base64Str) {
            var base64StrLength = base64Str.length;
            var map = this._map;
            var reverseMap = this._reverseMap;
            if (!reverseMap) {
                reverseMap = this._reverseMap = [];
                for (var j = 0; j < map.length; j++) {
                    reverseMap[map.charCodeAt(j)] = j;
                }
            }
            var paddingChar = map.charAt(64);
            if (paddingChar) {
                var paddingIndex = base64Str.indexOf(paddingChar);
                if (paddingIndex !== -1) {
                    base64StrLength = paddingIndex;
                }
            }
            return parseLoop(base64Str, base64StrLength, reverseMap);
        }, _map: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/='
    };
    function parseLoop(base64Str, base64StrLength, reverseMap) {
        var words = [];
        var nBytes = 0;
        for (var i = 0; i < base64StrLength; i++) {
            if (i % 4) {
                var bits1 = reverseMap[base64Str.charCodeAt(i - 1)] << ((i % 4) * 2);
                var bits2 = reverseMap[base64Str.charCodeAt(i)] >>> (6 - (i % 4) * 2);
                words[nBytes >>> 2] |= (bits1 | bits2) << (24 - (nBytes % 4) * 8);
                nBytes++;
            }
        }
        return WordArray.create(words, nBytes);
    }
}());

CryptoJS.lib.Cipher || (function (undefined) {
    var C = CryptoJS;
    var C_lib = C.lib;
    var Base = C_lib.Base;
    var WordArray = C_lib.WordArray;
    var BufferedBlockAlgorithm = C_lib.BufferedBlockAlgorithm;
    var C_enc = C.enc;
    var Utf8 = C_enc.Utf8;
    var Base64 = C_enc.Base64;
    var C_algo = C.algo;
    var EvpKDF = C_algo.EvpKDF;
    var Cipher = C_lib.Cipher = BufferedBlockAlgorithm.extend({
        cfg: Base.extend(),
        createEncryptor: function (key, cfg) {
            return this.create(this._ENC_XFORM_MODE, key, cfg);
        }, createDecryptor: function (key, cfg) {
            return this.create(this._DEC_XFORM_MODE, key, cfg);
        }, init: function (xformMode, key, cfg) {
            this.cfg = this.cfg.extend(cfg);
            this._xformMode = xformMode;
            this._key = key;
            this.reset();
        }, reset: function () {
            BufferedBlockAlgorithm.reset.call(this);
            this._doReset();
        }, process: function (dataUpdate) {
            this._append(dataUpdate);
            return this._process();
        }, finalize: function (dataUpdate) {
            if (dataUpdate) {
                this._append(dataUpdate);
            }
            var finalProcessedData = this._doFinalize();
            return finalProcessedData;
        }, keySize: 128 / 32,
        ivSize: 128 / 32,
        _ENC_XFORM_MODE: 1,
        _DEC_XFORM_MODE: 2,
        _createHelper: (function () {
            function selectCipherStrategy(key) {
                if (typeof key == 'string') {
                    return PasswordBasedCipher;
                } else {
                    return SerializableCipher;
                }
            }
            return function (cipher) {
                return {
                    encrypt: function (message, key, cfg) {
                        return selectCipherStrategy(key).encrypt(cipher, message, key, cfg);
                    }, decrypt: function (ciphertext, key, cfg) {
                        return selectCipherStrategy(key).decrypt(cipher, ciphertext, key, cfg);
                    }
                };
            };
        }())
    });
    var StreamCipher = C_lib.StreamCipher = Cipher.extend({
        _doFinalize: function () {
            var finalProcessedBlocks = this._process(!!'flush');
            return finalProcessedBlocks;
        }, blockSize: 1
    });
    var C_mode = C.mode = {};
    var BlockCipherMode = C_lib.BlockCipherMode = Base.extend({
        createEncryptor: function (cipher, iv) {
            return this.Encryptor.create(cipher, iv);
        }, createDecryptor: function (cipher, iv) {
            return this.Decryptor.create(cipher, iv);
        }, init: function (cipher, iv) {
            this._cipher = cipher;
            this._iv = iv;
        }
    });
    var CBC = C_mode.CBC = (function () {
        var CBC = BlockCipherMode.extend();
        CBC.Encryptor = CBC.extend({
            processBlock: function (words, offset) {
                var cipher = this._cipher;
                var blockSize = cipher.blockSize;
                xorBlock.call(this, words, offset, blockSize);
                cipher.encryptBlock(words, offset);
                this._prevBlock = words.slice(offset, offset + blockSize);
            }
        });
        CBC.Decryptor = CBC.extend({
            processBlock: function (words, offset) {
                var cipher = this._cipher;
                var blockSize = cipher.blockSize;
                var thisBlock = words.slice(offset, offset + blockSize);
                cipher.decryptBlock(words, offset);
                xorBlock.call(this, words, offset, blockSize);
                this._prevBlock = thisBlock;
            }
        });

        function xorBlock(words, offset, blockSize) {
            var iv = this._iv;
            if (iv) {
                var block = iv;
                this._iv = undefined;
            } else {
                var block = this._prevBlock;
            }
            for (var i = 0; i < blockSize; i++) {
                words[offset + i] ^= block[i];
            }
        }
        return CBC;
    }());
    var C_pad = C.pad = {};
    var Pkcs7 = C_pad.Pkcs7 = {
        pad: function (data, blockSize) {
            var blockSizeBytes = blockSize * 4;
            var nPaddingBytes = blockSizeBytes - data.sigBytes % blockSizeBytes;
            var paddingWord = (nPaddingBytes << 24) | (nPaddingBytes << 16) | (nPaddingBytes << 8) | nPaddingBytes;
            var paddingWords = [];
            for (var i = 0; i < nPaddingBytes; i += 4) {
                paddingWords.push(paddingWord);
            }
            var padding = WordArray.create(paddingWords, nPaddingBytes);
            data.concat(padding);
        }, unpad: function (data) {
            var nPaddingBytes = data.words[(data.sigBytes - 1) >>> 2] & 0xff;
            data.sigBytes -= nPaddingBytes;
        }
    };
    var BlockCipher = C_lib.BlockCipher = Cipher.extend({
        cfg: Cipher.cfg.extend({
            mode: CBC,
            padding: Pkcs7
        }),
        reset: function () {
            Cipher.reset.call(this);
            var cfg = this.cfg;
            var iv = cfg.iv;
            var mode = cfg.mode;
            if (this._xformMode == this._ENC_XFORM_MODE) {
                var modeCreator = mode.createEncryptor;
            } else {
                var modeCreator = mode.createDecryptor;
                this._minBufferSize = 1;
            } if (this._mode && this._mode.__creator == modeCreator) {
                this._mode.init(this, iv && iv.words);
            } else {
                this._mode = modeCreator.call(mode, this, iv && iv.words);
                this._mode.__creator = modeCreator;
            }
        }, _doProcessBlock: function (words, offset) {
            this._mode.processBlock(words, offset);
        }, _doFinalize: function () {
            var padding = this.cfg.padding;
            if (this._xformMode == this._ENC_XFORM_MODE) {
                padding.pad(this._data, this.blockSize);
                var finalProcessedBlocks = this._process(!!'flush');
            } else {
                var finalProcessedBlocks = this._process(!!'flush');
                padding.unpad(finalProcessedBlocks);
            }
            return finalProcessedBlocks;
        }, blockSize: 128 / 32
    });
    var CipherParams = C_lib.CipherParams = Base.extend({
        init: function (cipherParams) {
            this.mixIn(cipherParams);
        }, toString: function (formatter) {
            return (formatter || this.formatter).stringify(this);
        }
    });
    var C_format = C.format = {};
    var OpenSSLFormatter = C_format.OpenSSL = {
        stringify: function (cipherParams) {
            var ciphertext = cipherParams.ciphertext;
            var salt = cipherParams.salt;
            if (salt) {
                var wordArray = WordArray.create([0x53616c74, 0x65645f5f]).concat(salt).concat(ciphertext);
            } else {
                var wordArray = ciphertext;
            }
            return wordArray.toString(Base64);
        }, parse: function (openSSLStr) {
            var ciphertext = Base64.parse(openSSLStr);
            var ciphertextWords = ciphertext.words;
            if (ciphertextWords[0] == 0x53616c74 && ciphertextWords[1] == 0x65645f5f) {
                var salt = WordArray.create(ciphertextWords.slice(2, 4));
                ciphertextWords.splice(0, 4);
                ciphertext.sigBytes -= 16;
            }
            return CipherParams.create({
                ciphertext: ciphertext,
                salt: salt
            });
        }
    };
    var SerializableCipher = C_lib.SerializableCipher = Base.extend({
        cfg: Base.extend({
            format: OpenSSLFormatter
        }),
        encrypt: function (cipher, message, key, cfg) {
            cfg = this.cfg.extend(cfg);
            var encryptor = cipher.createEncryptor(key, cfg);
            var ciphertext = encryptor.finalize(message);
            var cipherCfg = encryptor.cfg;
            return CipherParams.create({
                ciphertext: ciphertext,
                key: key,
                iv: cipherCfg.iv,
                algorithm: cipher,
                mode: cipherCfg.mode,
                padding: cipherCfg.padding,
                blockSize: cipher.blockSize,
                formatter: cfg.format
            });
        }, decrypt: function (cipher, ciphertext, key, cfg) {
            cfg = this.cfg.extend(cfg);
            ciphertext = this._parse(ciphertext, cfg.format);
            var plaintext = cipher.createDecryptor(key, cfg).finalize(ciphertext.ciphertext);
            return plaintext;
        }, _parse: function (ciphertext, format) {
            if (typeof ciphertext == 'string') {
                return format.parse(ciphertext, this);
            } else {
                return ciphertext;
            }
        }
    });
    var C_kdf = C.kdf = {};
    var OpenSSLKdf = C_kdf.OpenSSL = {
        execute: function (password, keySize, ivSize, salt) {
            if (!salt) {
                salt = WordArray.random(64 / 8);
            }
            var key = EvpKDF.create({
                keySize: keySize + ivSize
            }).compute(password, salt);
            var iv = WordArray.create(key.words.slice(keySize), ivSize * 4);
            key.sigBytes = keySize * 4;
            return CipherParams.create({
                key: key,
                iv: iv,
                salt: salt
            });
        }
    };
    var PasswordBasedCipher = C_lib.PasswordBasedCipher = SerializableCipher.extend({
        cfg: SerializableCipher.cfg.extend({
            kdf: OpenSSLKdf
        }),
        encrypt: function (cipher, message, password, cfg) {
            cfg = this.cfg.extend(cfg);
            var derivedParams = cfg.kdf.execute(password, cipher.keySize, cipher.ivSize);
            cfg.iv = derivedParams.iv;
            var ciphertext = SerializableCipher.encrypt.call(this, cipher, message, derivedParams.key, cfg);
            ciphertext.mixIn(derivedParams);
            return ciphertext;
        }, decrypt: function (cipher, ciphertext, password, cfg) {
            cfg = this.cfg.extend(cfg);
            ciphertext = this._parse(ciphertext, cfg.format);
            var derivedParams = cfg.kdf.execute(password, cipher.keySize, cipher.ivSize, ciphertext.salt);
            cfg.iv = derivedParams.iv;
            var plaintext = SerializableCipher.decrypt.call(this, cipher, ciphertext, derivedParams.key, cfg);
            return plaintext;
        }
    });
}());

(function () {
    var C = CryptoJS;
    var C_lib = C.lib;
    var BlockCipher = C_lib.BlockCipher;
    var C_algo = C.algo;
    var SBOX = [];
    var INV_SBOX = [];
    var SUB_MIX_0 = [];
    var SUB_MIX_1 = [];
    var SUB_MIX_2 = [];
    var SUB_MIX_3 = [];
    var INV_SUB_MIX_0 = [];
    var INV_SUB_MIX_1 = [];
    var INV_SUB_MIX_2 = [];
    var INV_SUB_MIX_3 = [];
    (function () {
        var d = [];
        for (var i = 0; i < 256; i++) {
            if (i < 128) {
                d[i] = i << 1;
            } else {
                d[i] = (i << 1) ^ 0x11b;
            }
        }
        var x = 0;
        var xi = 0;
        for (var i = 0; i < 256; i++) {
            var sx = xi ^ (xi << 1) ^ (xi << 2) ^ (xi << 3) ^ (xi << 4);
            sx = (sx >>> 8) ^ (sx & 0xff) ^ 0x63;
            SBOX[x] = sx;
            INV_SBOX[sx] = x;
            var x2 = d[x];
            var x4 = d[x2];
            var x8 = d[x4];
            var t = (d[sx] * 0x101) ^ (sx * 0x1010100);
            SUB_MIX_0[x] = (t << 24) | (t >>> 8);
            SUB_MIX_1[x] = (t << 16) | (t >>> 16);
            SUB_MIX_2[x] = (t << 8) | (t >>> 24);
            SUB_MIX_3[x] = t;
            var t = (x8 * 0x1010101) ^ (x4 * 0x10001) ^ (x2 * 0x101) ^ (x * 0x1010100);
            INV_SUB_MIX_0[sx] = (t << 24) | (t >>> 8);
            INV_SUB_MIX_1[sx] = (t << 16) | (t >>> 16);
            INV_SUB_MIX_2[sx] = (t << 8) | (t >>> 24);
            INV_SUB_MIX_3[sx] = t;
            if (!x) {
                x = xi = 1;
            } else {
                x = x2 ^ d[d[d[x8 ^ x2]]];
                xi ^= d[d[xi]];
            }
        }
    }());
    var RCON = [0x00, 0x01, 0x02, 0x04, 0x08, 0x10, 0x20, 0x40, 0x80, 0x1b, 0x36];
    var AES = C_algo.AES = BlockCipher.extend({
        _doReset: function () {
            if (this._nRounds && this._keyPriorReset === this._key) {
                return;
            }
            var key = this._keyPriorReset = this._key;
            var keyWords = key.words;
            var keySize = key.sigBytes / 4;
            var nRounds = this._nRounds = keySize + 6;
            var ksRows = (nRounds + 1) * 4;
            var keySchedule = this._keySchedule = [];
            for (var ksRow = 0; ksRow < ksRows; ksRow++) {
                if (ksRow < keySize) {
                    keySchedule[ksRow] = keyWords[ksRow];
                } else {
                    var t = keySchedule[ksRow - 1];
                    if (!(ksRow % keySize)) {
                        t = (t << 8) | (t >>> 24);
                        t = (SBOX[t >>> 24] << 24) | (SBOX[(t >>> 16) & 0xff] << 16) | (SBOX[(t >>> 8) & 0xff] << 8) | SBOX[t & 0xff];
                        t ^= RCON[(ksRow / keySize) | 0] << 24;
                    } else if (keySize > 6 && ksRow % keySize == 4) {
                        t = (SBOX[t >>> 24] << 24) | (SBOX[(t >>> 16) & 0xff] << 16) | (SBOX[(t >>> 8) & 0xff] << 8) | SBOX[t & 0xff];
                    }
                    keySchedule[ksRow] = keySchedule[ksRow - keySize] ^ t;
                }
            }
            var invKeySchedule = this._invKeySchedule = [];
            for (var invKsRow = 0; invKsRow < ksRows; invKsRow++) {
                var ksRow = ksRows - invKsRow;
                if (invKsRow % 4) {
                    var t = keySchedule[ksRow];
                } else {
                    var t = keySchedule[ksRow - 4];
                } if (invKsRow < 4 || ksRow <= 4) {
                    invKeySchedule[invKsRow] = t;
                } else {
                    invKeySchedule[invKsRow] = INV_SUB_MIX_0[SBOX[t >>> 24]] ^ INV_SUB_MIX_1[SBOX[(t >>> 16) & 0xff]] ^ INV_SUB_MIX_2[SBOX[(t >>> 8) & 0xff]] ^ INV_SUB_MIX_3[SBOX[t & 0xff]];
                }
            }
        }, encryptBlock: function (M, offset) {
            this._doCryptBlock(M, offset, this._keySchedule, SUB_MIX_0, SUB_MIX_1, SUB_MIX_2, SUB_MIX_3, SBOX);
        }, decryptBlock: function (M, offset) {
            var t = M[offset + 1];
            M[offset + 1] = M[offset + 3];
            M[offset + 3] = t;
            this._doCryptBlock(M, offset, this._invKeySchedule, INV_SUB_MIX_0, INV_SUB_MIX_1, INV_SUB_MIX_2, INV_SUB_MIX_3, INV_SBOX);
            var t = M[offset + 1];
            M[offset + 1] = M[offset + 3];
            M[offset + 3] = t;
        }, _doCryptBlock: function (M, offset, keySchedule, SUB_MIX_0, SUB_MIX_1, SUB_MIX_2, SUB_MIX_3, SBOX) {
            var nRounds = this._nRounds;
            var s0 = M[offset] ^ keySchedule[0];
            var s1 = M[offset + 1] ^ keySchedule[1];
            var s2 = M[offset + 2] ^ keySchedule[2];
            var s3 = M[offset + 3] ^ keySchedule[3];
            var ksRow = 4;
            for (var round = 1; round < nRounds; round++) {
                var t0 = SUB_MIX_0[s0 >>> 24] ^ SUB_MIX_1[(s1 >>> 16) & 0xff] ^ SUB_MIX_2[(s2 >>> 8) & 0xff] ^ SUB_MIX_3[s3 & 0xff] ^ keySchedule[ksRow++];
                var t1 = SUB_MIX_0[s1 >>> 24] ^ SUB_MIX_1[(s2 >>> 16) & 0xff] ^ SUB_MIX_2[(s3 >>> 8) & 0xff] ^ SUB_MIX_3[s0 & 0xff] ^ keySchedule[ksRow++];
                var t2 = SUB_MIX_0[s2 >>> 24] ^ SUB_MIX_1[(s3 >>> 16) & 0xff] ^ SUB_MIX_2[(s0 >>> 8) & 0xff] ^ SUB_MIX_3[s1 & 0xff] ^ keySchedule[ksRow++];
                var t3 = SUB_MIX_0[s3 >>> 24] ^ SUB_MIX_1[(s0 >>> 16) & 0xff] ^ SUB_MIX_2[(s1 >>> 8) & 0xff] ^ SUB_MIX_3[s2 & 0xff] ^ keySchedule[ksRow++];
                s0 = t0;
                s1 = t1;
                s2 = t2;
                s3 = t3;
            }
            var t0 = ((SBOX[s0 >>> 24] << 24) | (SBOX[(s1 >>> 16) & 0xff] << 16) | (SBOX[(s2 >>> 8) & 0xff] << 8) | SBOX[s3 & 0xff]) ^ keySchedule[ksRow++];
            var t1 = ((SBOX[s1 >>> 24] << 24) | (SBOX[(s2 >>> 16) & 0xff] << 16) | (SBOX[(s3 >>> 8) & 0xff] << 8) | SBOX[s0 & 0xff]) ^ keySchedule[ksRow++];
            var t2 = ((SBOX[s2 >>> 24] << 24) | (SBOX[(s3 >>> 16) & 0xff] << 16) | (SBOX[(s0 >>> 8) & 0xff] << 8) | SBOX[s1 & 0xff]) ^ keySchedule[ksRow++];
            var t3 = ((SBOX[s3 >>> 24] << 24) | (SBOX[(s0 >>> 16) & 0xff] << 16) | (SBOX[(s1 >>> 8) & 0xff] << 8) | SBOX[s2 & 0xff]) ^ keySchedule[ksRow++];
            M[offset] = t0;
            M[offset + 1] = t1;
            M[offset + 2] = t2;
            M[offset + 3] = t3;
        }, keySize: 256 / 32
    });
    C.AES = BlockCipher._createHelper(AES);
}());

var key = CryptoJS.enc.Hex.parse("4587dc9b6a7c3e9ef3b920f994edc3a210c460977528138d41e58b9b02c94ffd");
var iv = CryptoJS.enc.Hex.parse("6aa677d0f4d6646eec5e9a82aedb60b0");

function AES_Encrypt(word) {
    var srcs = CryptoJS.enc.Utf8.parse(word);
    var encrypted = CryptoJS.AES.encrypt(srcs, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    return CryptoJS.enc.Hex.stringify(CryptoJS.enc.Base64.parse(encrypted.toString()));
}

function AES_Decrypt(word) {
    var srcs = CryptoJS.enc.Base64.stringify(CryptoJS.enc.Hex.parse(word));
    var decrypt = CryptoJS.AES.decrypt(srcs, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    return decrypt.toString(CryptoJS.enc.Utf8);
}
var data='eab8325abc5a1440b7708431e83f79acbb6d93e754ffb97db9a3e909336ebc6fa5c77d03767d43a902fe34554e069f3ef108ec973e20e97d0c32be7356d22a1f6e8ea837a9814ec86c6859fb3b96d3fb658ac67993f1beb71831fe406a0d26a2862bdc2b568b65980797ca725824759bab768cd65bc15fcc52ca58a02804456bbe1e7512fdd99a59fbd3b83f957f8250480999e1cbcbe148c99cfd11126d55371ccb563548ccd8df5587782dd7bc28eee2ef21d05fb580a861550283497ddd771692cc2f0d52f202b9368ba1f588f31097abca6503bfb6fc7a94181c21ff775448d62490d419ff52c0764f4a5a4086c31c12cdb3d6c49e5c100f4fc50db3ee3f069df5f3c0f5e2508261b2522240c5db187136d784ce1b71dfa8929271c22c8786e5cc96e5f810b11eff50ade8b8d88ce2c8897bd20bf07edc08a113344d313b8793288a62c389f307e49b52a86cc7c7180e805e375c338734052ea92a790b1d1461f9ff43617662256f616f73b65e1578fe5d76de4f9267497aaaad11d315b686e57c0d30d9ff0e16815cfe86315108f85de0c86096eb839dc68501874e671daaec503be3a02c8acde0f9be9948ca59e64bd9a7ecdfc62d2e431293c18844b5cc9ee07b7ac8135a69c2519b764d58a5363101a87d0c01400953bdc9927960956d2d93b824025248b02be7f00326e42ab37dcb9fb9a1b878eb5872249b6ed01a4662263d4ebc77e27be90cba3e06b5507c27736100847ea4ef4fff5517f5ecfc0149c3629c8d07c33a5f82467ae05013100af71070d3ca4197cf1a78151d809f675fcb2dee5609458d9a7c897c10cfe9a208abd05112b758c9c1a3fb8dba74972756dda008a5421443ca0e43be22dac7ac7bb6dc5d4f4e5fd3e1da327385fbffded88345fb66ef79fbcdcb7c3cb41232e7b4a06ae87d5c46e81efae06a86ddaa30d6e8e89a5c5ef7a26f96654fc4ffe0ce2496ab1c63d004502371626f2db6988002fc965b5cbcbfe1fc5d15ea396754c893df2fa3907cf45ea02afc3468a2646bd950e05b5ea8be736e7278d0891a1f2373a126b1267c19e5f9043825eed12d10d89b848bff44a44fb473cb425461bb67ba784d098060242df0605b6d62fe9fd1e7d832921124a0dde051e7b7d52630629523a817f09f7fac73d6d3fa59b727835eb0ae715a9f3f976c075acb616d1df462c56cdf494bb9fb320db118e8c85a132adda817cfcdf3d31d74b4a011f4f588bc17d296631e3d81ca37330c75e39d02b4f801b6fb73f0e77e2c4fe355b911ac2d6ede9edbec8a5587300557cb090fa50772fd8ab1b66b9ce68642d7326282001fec8c8f60d0864f543b8f4a4190803b954dd251a5e4cb1ddf8afdf1aef21203c06ebb88101c33bf70929d88f989112b653e71207d95ea5aad995a9634b9f2fb5f5180de0999cc0baee0286532aaa6cdcf3ff3719bd0934a355854881c08a01bff50fc031d5eab22155958677eebdd332a91435017c9c5260a54bf8d6461368fc5a28b0f0b12731592aae1caa21f59236df5516f2bf079da2d13fed406389c584e03232dd4fbf8b944476b9bacd4f3b4394af8651f03b81aec237527df8c6d45f2653b23685d08789e5d47d9e8117979a5d904225cdf48dbd25a2b7c61c1be507a6f322ecb823b4f7b62686914556957602075b13be2dac9af3bf7fe22b361bd3963724084cfdbca957134f32f75a1f0e1f63ff640999d16ad7675ab5da48d6c49d5e85e4bd5dddf2620c29f313add184c0123e587fe03d0d0833175e2c05c9cc3794934ae0c8dac883147b2f8bf0e8b44588f6b539c227c12aee61146c7d83f861fe7ebc1a8c549c108627d7bdbc9a77c41e43aea45f6473cf8c92738fe5547f29aee6e9969afb7fb1b17bc6a8fffb2a0e23c97f66d025c0670ef5b45d2b0506e5c09469e43814d1f9f520dd0c3f80184e7d7c89aa6cd9451e192685c18c63053ef61cf7711e4fc0104d0238d2946ed5a523d06674c76e6e6548271b6243d8a1adc220d5330362534b2f0fd9a787c84761c466d7b083a8fc7f902c2af18c02661e16af6b0a35224f0728ceb779e27216eb83a7f576aab0818ed2065310e06dee0c56416915c9d4eee5e268ea169cba8e969434e2181067d17eaa2d7691b130e20b7c9c39f51ccb0e834f97938a45ac63abfa1fc17ab6d06aa2588fd9a4159f2e00a744a7fd6d10105f9adb797984504df3d5228a814ee481521d6172e11a9266a55340acadfeb290c894d529f1abf5d885163e9d07f29eedd2828becaa23eed25aa858e26c9e82052d882589decbd3aa8721cf972b4237811831006788f9637a7769ee791603e595d07930d0e8f3391c626f665e7cfad4b83f1cfda8a10615faeb6bfc3042a1365daded79cff122db62f9bda995d88e8a9d9748e34e4f08b0dcb19d00135aa082f6a82af4f63feeff76af878dde25d792939035465c1a601f0e41dd7b2b37f6fc3e39fb105b9e94dd839257a0782621010538eca656aedf386fb2a136c2ecba35b32d537edb110b974eca7b7345b525fd5b679d741389e15d910d8f8b76e5f8776e017913c32a3384cceafad190bdc09b398c5b8cb259015f08ea4d8d5e27ca1d2b7930b7bbfcea03a449958f835f032ce85b41c82ada0195a047afe872085f6d2a11c4a7b6765c80aed6a2293118d3d478debd7401f7c07082b529bc70dfea042fac0222588e9b3dea91f16804c8e12ac8e2ac084f374477a9b60ef02c97cb1e136b1a7bff6ed0505216abeeb8b60e0fa6cd05383129c01c1b8643a648294199a71d40956a22879dffce5a16fe3d189c3fda64c82b3251d64e67e5286f9182192e2897f922f1ca5194a35e7134dc0f9f74be0300bce46ea90bc0a598ebf52522bf264dbceabec45813814cfd94581df49c5d5fc4222158dcf33d443000d22f6b9ab27b97d4a2571e16ec53703806072868a34b5c8b501624df2eaecaaff1f228158815c2c9fe9960ea20a21cd5e980e2b3c6121468adc90e9a3d1229989d2f4ec89e107d39cd3f6a6de5c5e38bfc6419b97fdeccbdad1b796eaf3d3c9b92ff62d1d55d86ec52a2d0ca92a6fc9a00818b883df5f96723bc1bc8063b664260df4b678cab9c05dda1060aff946e7104c5a4ded8b68bfb96ec7556fa5d1d9f617e41ac4c486671bb89bbd53f55fd34adc6965ee8c9afe3e2ad1ed06479130f0d758ca336326210090e2015aa2be5798d604b8074233c8d099c8e1b101c7f96c78306257e4973044af48110f326770ceeed2b35d87a3f6ce3fc96f97187605101654996c3338c82de4ac8734159cbac1a4847292ad030d398d5dc7351276ef23585ea52b1fbbe723394b91ef303f38bad03ab0b9b2a787124a34eb799c448402a70100aa7350cd3a58c6931f94179117c64b2a8c86928d614835f4c1bc29c546d913932cb17b78c1bac3c8dcd03130cbdd3889f0b5b46e7ff8c2007e5bbf6689049eae153fb93574a7134f2ccb4f1df1387e1baa1141b4ccb8d56ba764bdb73fd483a131bb1c2843fe12323e3692cc373d46664957e74d36ccd8e308636670b625549ecd103d4d51960215dc4b7d34fd035400e4cbaf0a98419e32ba9509c5738d01a4651c033b9b53a093327204cb45667cdc3ecacd75cc60a2abaf7665f4a6c091489c99dee08ee12e8736a17dbe000c9a78a4450e88d112db1215e6148dd1debfc91242289e90a303b280fb5f5ba12f43c45909f56a9eca8affe9f36560949d8abdb8e362c666efcf56f390c8ec48e0f6e9380dcc9fc62b55fb9cd7c2177ada9b8e73ed4fd6f8eff5fb5170507e4ec16438947c2c5b42082ed000d2414e7141b4a4f8443941433812b89ebdff6cdbaafcfc65ed1357ae4d11f23535b77e859fa313a1f97c9aa7038770cade727ea9341e5c6455e9aa0ab04af3b44c75ae16c4f6518630997774129c9b688206c0f4d1c79c12774552ecc27d8317f7f8f48c67cb24b84537be0a37fabf69ae995267ff70752d893dc09d2ed4967dc48ec7e64a9e36af8f97a957bb1266d5764d8277cdcc634f6ddda6473daf8cd7fc3efca9dfdd997b90a2961b7ac71b839a2f40b59daeef9a9b3e153a8f6f26bd956e7433635ba554c40c2bf6f7a3747dd539c930bf951bf09bf96660d51efa7ca291a053592a834b608b1b645c94b8dd7fcff79ae7bebe33c8ad5ca10e138ca054223edc5778ac62e6d38dd61dddca4a298ee4f2dab1f8a45828852156165ddad93dacfbadacfef92bfc5cc49c8f54b55fa029b21707a3dba0cb820cc38c1e457bc2766e0d699c6e5470ab0094bce377361abd8abc6272e93a743856385d5ed570dcc40a9704ff46d27ceb1612808068f57bc1bda7b03f64446058b60bc618a6b83c19ef1f20e3bf1b4a3ebd13fa2be0f34d6f03823f30644850c2635ee2baa4347dad5b677660635000b316f36c157c3713cc1fb8d922b6abf64790d73e5899f6dfeca58df1e4c2f37bd1473ba1df60e18bcd496150f2cd4ed859c9058ffbe1dcbc29870bc7043e7587694e6de3263a7c66db983a90e85922ffea3f5ddb746bdd26ad24f2bd4c3e1670c76ac6cdde0335e14fe741223cf216829532de108fa72493ff60ecd2f32bb7bd35e06a1a01ca25456cd8a9ca669fd0cc977ca2d1633be1de6a9b9086eed70e6f5b750657c852da4dc914b3049540014242b473e9e677afba3248eced6cb30357e6858e1967781641b25f0d0639c519642fd464f3e08e9a5e275cc943c391a9c18ec616b196166175e45030146b27b51608882f24e6bb2edb33bcfcfbd2bead8a54ddfe8793bd607f4dbf5d8180f2e0a5488f8c51669c1132dd8a1ed86dad3bf4f4fb14cd6b3d128f8fb54a55df88d09832f1313f237d5b43c9fcb6e5e31164a81eb579430f97dd39e5eba80a9f1cf05af2b623c64bd6146d4f6a980176fa30bb47a626e15f44b636bec735b41a404c1e7a279b931af69b9b1b7746bef945c38c1917085a195c9ef7c8d29c4c1d4e2e6c5443d4cc6fd13fc1bdf849a59ad3df1ea57ab0a5f066a0821ee3fa899d6d70720b3b31d2d268d5f7bfe5dc5d5a1dd44a13fead05cb53638d783939592dfe19ebf52a06afb37b1c885906d992b19576d4fb4bf85f892eaa515bc1791c1c5ae5101e91a93ea6204baeae7b45e531f1460062508d5d4a09ba4e63ce55128b7fecb5f81e61f51e23e4185e968109608e2c594903a4525e4cc8eb35043be7a2399f53edd775a78e43325dbd064219511a0683ab356ebe9cdd0d9da7f85fbee24d2a3d32cb8365382b9b2578aa93cbb5bd00e9b6921502e02af6b4439b1c2cf36cb9597ced751f6c6d490645c2af91115e82530a60c79fa71f88fbd6a7c8412f934374645c1588e39dc827d54f806152d89190d9889e46455483092c22a5d8de2e78ad7f140199ef7dd19d6d92de3c649c9fbb19f6f68d4b786855b022bcb4a453a2bd32db10fbbe58b7c665552d81303069b3fa49a49a0c51e989d857dc49d1ff7e7201177d610a8419e94889c4693c13934dafff162be6e3830695e55d60aa44840e3bc61233128ff2b0bb834cda423cc10dc7ac36a7747aa778025b9716e71f49321b95e6490a9223387ecb4c0c41a66b4ea516f3400048443d6b217ecfa1fa4b89cad746fce2bebef7204cd38ae4b1b021bbab60b928d3ab999c3cf4291495419b8cb001d5b1324d00a35e84abd29f3c8e71f183010ae3b7594560ea06a8749600b7c0e596809fa04078da260a02a78ec0fb918908d7148b0acc8d8ec1637aba9b4ad48e1e6358a8c8887e164f3135458bf623201b102ad06079a24a8b87f7e276c645e7cff61603414e565606bddd467da66aebb4d980802ebdd19d41ea924222c6848dc75259d2bbcd3ff3794f398d9de6a63f2e0bf2e30561650f64893a7daf926b42f928f625415bfc14cf524ff713b84408db4c3f7013af976672d486e48b20e9bf99f1d65d32ee0f90998e56ab9156d4f3078d68afbdbb61e259ee554da240eacd6bb974efe4b00429b1f9bf9dce5cbf2256019a7c0bb0772a7690d8fed79cbbf8376ed3791e8b17de6197ba8bb6002366a22577d4d57cc9ee1096e405bdaac9e77d91515269cb0a0fd6b5599fa7bd74bf89d974b69834b5505b58d00a1edba9b693a93507669c997835b7866926c97276af8640bf10663a30ddd8201a1b4bd3c562ad945bc699ebc57dfaa9a79f00d458d3ad82371bca11d57f3fbaa0776f230c0463309196400d07da1c46df36605610c4234fe4ea24cfad116c6b935ce9f6c9447d11c7cd0571639ac6a12786610705c30032376fed8eaa94fca700610ad0903052a89e04b3c8dee60e16f0f6bf479913dc17227d59e497d6e5cc551ec4ef6b41259a88c7e090d74f67a84245b768d11938aaa59e91097bc3adca12b1c066f6834634514184aa98f005b697d136f9ae47b2676d039ec8a54a0aacba8c2de4315d9539b1448e07484388b20f88edc327d505eb3d66371adcc469ad317eb9d0ec9aa26cbca2dec6b9542e8039ec8e298bbc5dfc7c98a07ea46fc95bde8e080da256d84e75f84b15a8a0a0d341149a1404ab6b4fc0fd6e0d03be9aad96d898a3ada0bc58800b6344c3912e881544344a4fea4a070e97e3b59b23cb0490566aa403fe87ec50deea84530dd4ac6ef238fb6ab44539ba5184710d50ef6bfe48cf448021b466a90d987812473da645239d07229539acf1993b76b2c12a48f23973ec1a324e7660a4da3a272e085c3eeb38d7a3cc8de7a44e93d1bdcebc3feb75ce9a537f20e437fd2f7ab70145415c09bd56ca380a3d3011d2e96cb3bcc74c7571a5e3704ad3b6dee3efbbbb9c06ad713fbd5bc3ad12a6e74b1d9fc250d96a86802056c102ba5178b1c3bc32823980f3348e2a6c46e7018da6b2be484ff5b8033332807a90b6428e293135b1199267e7e447000a8e9ceda12313d363b9953a4532da76b861cd3202cd4eef86c20e25b4fe5bdd07333ca14398d0d976cca91033f223b09be37ca9969afdbecdef7d3b0c4743a931d06feea202c70fd1d943288d377192f9e16e40499936eb70304e416821f5818cf5d799a4e2f6a6537a1714f2ff9b0be12561f5e059983ae2d03672fbbcb857b22565f8c6a286ca1c6d4a055b9265e02842cc9d4d37c5fe95766051c42a2b975c21479c810b2859b8863c0ad97e8b2fd08a46363c702dc88760567c8bd5fadb87940acc4f4e52c4ba035b75b9abafbee7ac92c7e52aa8572c2c259840b66981053fd701f8a1a1f2304c9ab39343659def12248718d9a39aa551499cb52d36e415cab6f016b56c2d8e20b49d2ed52d78f0d34c539eefbfeff245c67b3557d9d76fb0ef1d29172e3d674dcbf72067f69ff223bbb1991dff6ea79f35ffaae5f55486322dd6920c75fae42733d5b4b45e99cf26fd7ab7fc41845208dcc7d0c48a732d31a5d041881b3b92ab4400382669fb44e6723662148d96ab9dd7680f53fc4a35678fcf4e1c4eedad8011bb2c8650a3551736928dc1518f84d032458a1a1bc63517e672b42f444a8c7d18e8b9a1883d52954b67151b30b52bf53a40ed5efc1b3df1ef0b60351109c4c6335a2793344e762021a773feae5f81671c4f37e538fadace66c9d265b20d6758c8dfbfbcd8b953fc9f1ce77922bb7953f8d09490ef3dc48a4e84fcf1416408405814bf3858819bb0623a4a224cc800a615d21540adc3350d666d54f78a3535b283ce984455b883b62a82967b0eb2808fdac0c0b14965dae8b9b6df9117629a6ad8207aa7404f86c5019957a89806a981bf8f6a32066ef911d0135fab0a602d4715c826dfa45f31dd16edfafdf60be0860b6a634395ae4d8cf95072e21560171b605e94a73afa84658184122f95b7d4d265446c21fa9d6af6079a11b04ac01dcc42a787ce5bb5dcd2118d51e19260d4da566e70c056d3596b83c0336dd017ad2301d235bc1070849ba8c39d4c99486a805af94ad9eba03d0a4c256eb036335d8e33e0c105a2305a734ea9c38bed6613936efc60a9691936505f76705391c96bd5e66933375937a44f860f5f611954a465a0f657f32a6348501621bc0952a6102ff8257235fe4dc42f03267c7361d28c124e084d6ceab58f4e3992619dd49bbf3dbdb07a4164057302c2701d0f004691231e487746e8ced936b783ffeae8ba07491c69cf0fa4ac3163d09d82ba971523acc995bef28904eaff9483ec383001dd30f2a964aa6ed007e02ebadfbc53daab18e3a2120eb9b739ad40793f72199645890d58027aedb3526f4b89670c0bb78054d2acf7f6f9fd6b2b5209ce402008c2915c8113bac013366ca9b5b348305629130d0a3ab9f22d3bd81f5684034afa6127a13f19fd8e3602754012b6da100147b12850a6738d49c09956b8384e383950d5931948d9bb8b6bc01c92c43f88577b86835a571a872762e004fefe5975cc142719396a416636cd292778d4da6cc2dedc866d5c9526ba9eaded9ad3dd8ca1c500c3bffc35531a0b18fd7ef1d4698a9dd718f4119d40647fad03b20d7d84aa35db532fbee6641cc722b2aaab22761bf0f4f0fb0296855f121986ef12f1cb6d4fd342dbbdb6d016587d18f93f8d1ddfe7cc1d4cd93bb8a216743f694f555ffae9c782277ca880fc94a92c1448debbc1b2229ea609ad5b101fe3e46c96330c891d3d3167aea26254862aff1be5c74c15a3ba79dbcd34f5cb2d1265d6595b92ff691633534f856473d9c3214daa5fb042e64ac72cb5b6730e0e010e705ccdd3c62fbe77eb62187388ff46ba0c072210f40e46c271978f956076e08befa6aece0205948fb760dc0af18154145dc7e805ff066e5a430e517ce9e4df6f1768a61717686c94941cc8e0d427e13ce5e256c5e7b8a943de54ec999b2b8025af3384c36091e88a269bfc865486';

function get_data(){
return AES_Decrypt(data)
}
console.log(get_data())
```

> 二次重写

```
var CryptoJS = require("crypto-js");

var key = CryptoJS.enc.Hex.parse("4587dc9b6a7c3e9ef3b920f994edc3a210c460977528138d41e58b9b02c94ffd");
var iv = CryptoJS.enc.Hex.parse("6aa677d0f4d6646eec5e9a82aedb60b0");

console.log(key);
console.log(iv);

function AES_Decrypt(word) {
    var srcs = CryptoJS.enc.Base64.stringify(CryptoJS.enc.Hex.parse(word));
    var decrypt = CryptoJS.AES.decrypt(srcs, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    return decrypt.toString(CryptoJS.enc.Utf8);
}

url = ""
data = "b9a35487d43ec989030037d14d27e154f69d28fc3d2ac20899b0c6479d652077"

console.log(AES_Decrypt(data))
```

> 最终完结, 又因为偷懒 想直接部署到服务器  
> 使用 express 部署 (第一次写 代码像 ? 一样)

```
npm install express   
npm install body-parser 
npm install cors --save
npm install crypto-js
```

> index.js

```
/* 引入express框架 */
const express = require('express');
const app = express();
/* 引入cors */
const cors = require('cors');
app.use(cors());
/* 引入body-parser */
const bodyParser = require('body-parser');
const CryptoJS = require("crypto-js");
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}));


const {get_data} = require("./data_decode.js");

app.all('*', function (req, res, next) {
    if (!req.get('Origin')) return next();
    // use "*" here to accept any origin
    res.set('Access-Control-Allow-Origin', '*');
    res.set('Access-Control-Allow-Methods', 'GET');
    res.set('Access-Control-Allow-Headers', 'X-Requested-With, Content-Type');
    // res.set('Access-Control-Allow-Max-Age', 3600);
    if ('OPTIONS' == req.method) return res.send(200);
    next();
});

app.get('/', (req, res) => {
    res.send('<p style="color:red">服务已启动</p>');
})

// get接收前端传递过来的参数
app.post("/api/post/signsafe",(req,res)=>{
    let url = req.body.url; // 接收前端传递过来的参数
    console.log(url)
    let obj = {
        code: 2000,
        status: 'ok',
        data: signsafe_jami(url)
    }
    res.send(obj)
})

app.get("/api/get/signsafe",(req,res)=>{
    let url = req.query.url; // 接收前端传递过来的参数

    console.log(url)
    let obj = {
        code: 2000,
        status: 'ok',
        data: signsafe_jami(url)
    }
    res.send(obj)
} )

app.post("/api/post/data",(req,res)=>{
    console.log(req.body);
    let data = req.body.data; // 接收前端传递过来的参数

    let obj = {
        code: 2000,
        status: 'ok',
        data: get_data(data)
    }
    res.send(obj)
})

/* 监听端口 */
app.listen(3000, () => {
    console.log('listen:3000');
})



function signsafe_jami(p) {
    p = p.replace(/^\/|https?:\/\/\/?/, "");
    p = p.replace(/%2F/g, "/");

    console.log(p)
    let f = CryptoJS.HmacSHA1(CryptoJS.enc.Utf8.parse(p), "D23ABC@#56");

    f = CryptoJS.enc.Base64.stringify(f).toString();

    f = CryptoJS.MD5(f).toString();
    return f;
}
```

> data\_decode.js

```
exports.get_data = AES_Decrypt

var CryptoJS = require("crypto-js");

var key = CryptoJS.enc.Hex.parse("4587dc9b6a7c3e9ef3b920f994edc3a210c460977528138d41e58b9b02c94ffd");
var iv = CryptoJS.enc.Hex.parse("6aa677d0f4d6646eec5e9a82aedb60b0");

console.log(key);
console.log(iv);

function AES_Decrypt(word) {
    var srcs = CryptoJS.enc.Base64.stringify(CryptoJS.enc.Hex.parse(word));
    var decrypt = CryptoJS.AES.decrypt(srcs, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    return decrypt.toString(CryptoJS.enc.Utf8);
}
```

### 最后记录一下最开始的解密 data 代码 ()

> Tips: 只能说 可以实现 但没必要

```
exports.get_data = get_data

var CryptoJS = CryptoJS || (function (Math, undefined) {
    var C = {};
    var C_lib = C.lib = {};
    var Base = C_lib.Base = (function () {
        function F() {};
        return {
            extend: function (overrides) {
                F.prototype = this;
                var subtype = new F();
                if (overrides) {
                    subtype.mixIn(overrides);
                }
                if (!subtype.hasOwnProperty('init') || this.init === subtype.init) {
                    subtype.init = function () {
                        subtype.$super.init.apply(this, arguments);
                    };
                }
                subtype.init.prototype = subtype;
                subtype.$super = this;
                return subtype;
            }, create: function () {
                var instance = this.extend();
                instance.init.apply(instance, arguments);
                return instance;
            }, init: function () {}, mixIn: function (properties) {
                for (var propertyName in properties) {
                    if (properties.hasOwnProperty(propertyName)) {
                        this[propertyName] = properties[propertyName];
                    }
                }
                if (properties.hasOwnProperty('toString')) {
                    this.toString = properties.toString;
                }
            }, clone: function () {
                return this.init.prototype.extend(this);
            }
        };
    }());
    var WordArray = C_lib.WordArray = Base.extend({
        init: function (words, sigBytes) {
            words = this.words = words || [];
            if (sigBytes != undefined) {
                this.sigBytes = sigBytes;
            } else {
                this.sigBytes = words.length * 4;
            }
        }, toString: function (encoder) {
            return (encoder || Hex).stringify(this);
        }, concat: function (wordArray) {
            var thisWords = this.words;
            var thatWords = wordArray.words;
            var thisSigBytes = this.sigBytes;
            var thatSigBytes = wordArray.sigBytes;
            this.clamp();
            if (thisSigBytes % 4) {
                for (var i = 0; i < thatSigBytes; i++) {
                    var thatByte = (thatWords[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                    thisWords[(thisSigBytes + i) >>> 2] |= thatByte << (24 - ((thisSigBytes + i) % 4) * 8);
                }
            } else if (thatWords.length > 0xffff) {
                for (var i = 0; i < thatSigBytes; i += 4) {
                    thisWords[(thisSigBytes + i) >>> 2] = thatWords[i >>> 2];
                }
            } else {
                thisWords.push.apply(thisWords, thatWords);
            }
            this.sigBytes += thatSigBytes;
            return this;
        }, clamp: function () {
            var words = this.words;
            var sigBytes = this.sigBytes;
            words[sigBytes >>> 2] &= 0xffffffff << (32 - (sigBytes % 4) * 8);
            words.length = Math.ceil(sigBytes / 4);
        }, clone: function () {
            var clone = Base.clone.call(this);
            clone.words = this.words.slice(0);
            return clone;
        }, random: function (nBytes) {
            var words = [];
            var r = (function (m_w) {
                var m_w = m_w;
                var m_z = 0x3ade68b1;
                var mask = 0xffffffff;
                return function () {
                    m_z = (0x9069 * (m_z & 0xFFFF) + (m_z >> 0x10)) & mask;
                    m_w = (0x4650 * (m_w & 0xFFFF) + (m_w >> 0x10)) & mask;
                    var result = ((m_z << 0x10) + m_w) & mask;
                    result /= 0x100000000;
                    result += 0.5;
                    return result * (Math.random() > .5 ? 1 : -1);
                }
            });
            for (var i = 0, rcache; i < nBytes; i += 4) {
                var _r = r((rcache || Math.random()) * 0x100000000);
                rcache = _r() * 0x3ade67b7;
                words.push((_r() * 0x100000000) | 0);
            }
            return new WordArray.init(words, nBytes);
        }
    });
    var C_enc = C.enc = {};
    var Hex = C_enc.Hex = {
        stringify: function (wordArray) {
            var words = wordArray.words;
            var sigBytes = wordArray.sigBytes;
            var hexChars = [];
            for (var i = 0; i < sigBytes; i++) {
                var bite = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                hexChars.push((bite >>> 4).toString(16));
                hexChars.push((bite & 0x0f).toString(16));
            }
            return hexChars.join('');
        }, parse: function (hexStr) {
            var hexStrLength = hexStr.length;
            var words = [];
            for (var i = 0; i < hexStrLength; i += 2) {
                words[i >>> 3] |= parseInt(hexStr.substr(i, 2), 16) << (24 - (i % 8) * 4);
            }
            return new WordArray.init(words, hexStrLength / 2);
        }
    };
    var Latin1 = C_enc.Latin1 = {
        stringify: function (wordArray) {
            var words = wordArray.words;
            var sigBytes = wordArray.sigBytes;
            var latin1Chars = [];
            for (var i = 0; i < sigBytes; i++) {
                var bite = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                latin1Chars.push(String.fromCharCode(bite));
            }
            return latin1Chars.join('');
        }, parse: function (latin1Str) {
            var latin1StrLength = latin1Str.length;
            var words = [];
            for (var i = 0; i < latin1StrLength; i++) {
                words[i >>> 2] |= (latin1Str.charCodeAt(i) & 0xff) << (24 - (i % 4) * 8);
            }
            return new WordArray.init(words, latin1StrLength);
        }
    };
    var Utf8 = C_enc.Utf8 = {
        stringify: function (wordArray) {
            try {
                return decodeURIComponent(escape(Latin1.stringify(wordArray)));
            } catch (e) {
                throw new Error('Malformed UTF-8 data');
            }
        }, parse: function (utf8Str) {
            return Latin1.parse(unescape(encodeURIComponent(utf8Str)));
        }
    };
    var BufferedBlockAlgorithm = C_lib.BufferedBlockAlgorithm = Base.extend({
        reset: function () {
            this._data = new WordArray.init();
            this._nDataBytes = 0;
        }, _append: function (data) {
            if (typeof data == 'string') {
                data = Utf8.parse(data);
            }
            this._data.concat(data);
            this._nDataBytes += data.sigBytes;
        }, _process: function (doFlush) {
            var data = this._data;
            var dataWords = data.words;
            var dataSigBytes = data.sigBytes;
            var blockSize = this.blockSize;
            var blockSizeBytes = blockSize * 4;
            var nBlocksReady = dataSigBytes / blockSizeBytes;
            if (doFlush) {
                nBlocksReady = Math.ceil(nBlocksReady);
            } else {
                nBlocksReady = Math.max((nBlocksReady | 0) - this._minBufferSize, 0);
            }
            var nWordsReady = nBlocksReady * blockSize;
            var nBytesReady = Math.min(nWordsReady * 4, dataSigBytes);
            if (nWordsReady) {
                for (var offset = 0; offset < nWordsReady; offset += blockSize) {
                    this._doProcessBlock(dataWords, offset);
                }
                var processedWords = dataWords.splice(0, nWordsReady);
                data.sigBytes -= nBytesReady;
            }
            return new WordArray.init(processedWords, nBytesReady);
        }, clone: function () {
            var clone = Base.clone.call(this);
            clone._data = this._data.clone();
            return clone;
        }, _minBufferSize: 0
    });
    var Hasher = C_lib.Hasher = BufferedBlockAlgorithm.extend({
        cfg: Base.extend(),
        init: function (cfg) {
            this.cfg = this.cfg.extend(cfg);
            this.reset();
        }, reset: function () {
            BufferedBlockAlgorithm.reset.call(this);
            this._doReset();
        }, update: function (messageUpdate) {
            this._append(messageUpdate);
            this._process();
            return this;
        }, finalize: function (messageUpdate) {
            if (messageUpdate) {
                this._append(messageUpdate);
            }
            var hash = this._doFinalize();
            return hash;
        }, blockSize: 512 / 32,
        _createHelper: function (hasher) {
            return function (message, cfg) {
                return new hasher.init(cfg).finalize(message);
            };
        }, _createHmacHelper: function (hasher) {
            return function (message, key) {
                return new C_algo.HMAC.init(hasher, key).finalize(message);
            };
        }
    });
    var C_algo = C.algo = {};
    return C;
}(Math));

(function () {
    var C = CryptoJS;
    var C_lib = C.lib;
    var WordArray = C_lib.WordArray;
    var C_enc = C.enc;
    var Base64 = C_enc.Base64 = {
        stringify: function (wordArray) {
            var words = wordArray.words;
            var sigBytes = wordArray.sigBytes;
            var map = this._map;
            wordArray.clamp();
            var base64Chars = [];
            for (var i = 0; i < sigBytes; i += 3) {
                var byte1 = (words[i >>> 2] >>> (24 - (i % 4) * 8)) & 0xff;
                var byte2 = (words[(i + 1) >>> 2] >>> (24 - ((i + 1) % 4) * 8)) & 0xff;
                var byte3 = (words[(i + 2) >>> 2] >>> (24 - ((i + 2) % 4) * 8)) & 0xff;
                var triplet = (byte1 << 16) | (byte2 << 8) | byte3;
                for (var j = 0;
                     (j < 4) && (i + j * 0.75 < sigBytes); j++) {
                    base64Chars.push(map.charAt((triplet >>> (6 * (3 - j))) & 0x3f));
                }
            }
            var paddingChar = map.charAt(64);
            if (paddingChar) {
                while (base64Chars.length % 4) {
                    base64Chars.push(paddingChar);
                }
            }
            return base64Chars.join('');
        }, parse: function (base64Str) {
            var base64StrLength = base64Str.length;
            var map = this._map;
            var reverseMap = this._reverseMap;
            if (!reverseMap) {
                reverseMap = this._reverseMap = [];
                for (var j = 0; j < map.length; j++) {
                    reverseMap[map.charCodeAt(j)] = j;
                }
            }
            var paddingChar = map.charAt(64);
            if (paddingChar) {
                var paddingIndex = base64Str.indexOf(paddingChar);
                if (paddingIndex !== -1) {
                    base64StrLength = paddingIndex;
                }
            }
            return parseLoop(base64Str, base64StrLength, reverseMap);
        }, _map: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/='
    };
    function parseLoop(base64Str, base64StrLength, reverseMap) {
        var words = [];
        var nBytes = 0;
        for (var i = 0; i < base64StrLength; i++) {
            if (i % 4) {
                var bits1 = reverseMap[base64Str.charCodeAt(i - 1)] << ((i % 4) * 2);
                var bits2 = reverseMap[base64Str.charCodeAt(i)] >>> (6 - (i % 4) * 2);
                words[nBytes >>> 2] |= (bits1 | bits2) << (24 - (nBytes % 4) * 8);
                nBytes++;
            }
        }
        return WordArray.create(words, nBytes);
    }
}());

CryptoJS.lib.Cipher || (function (undefined) {
    var C = CryptoJS;
    var C_lib = C.lib;
    var Base = C_lib.Base;
    var WordArray = C_lib.WordArray;
    var BufferedBlockAlgorithm = C_lib.BufferedBlockAlgorithm;
    var C_enc = C.enc;
    var Utf8 = C_enc.Utf8;
    var Base64 = C_enc.Base64;
    var C_algo = C.algo;
    var EvpKDF = C_algo.EvpKDF;
    var Cipher = C_lib.Cipher = BufferedBlockAlgorithm.extend({
        cfg: Base.extend(),
        createEncryptor: function (key, cfg) {
            return this.create(this._ENC_XFORM_MODE, key, cfg);
        }, createDecryptor: function (key, cfg) {
            return this.create(this._DEC_XFORM_MODE, key, cfg);
        }, init: function (xformMode, key, cfg) {
            this.cfg = this.cfg.extend(cfg);
            this._xformMode = xformMode;
            this._key = key;
            this.reset();
        }, reset: function () {
            BufferedBlockAlgorithm.reset.call(this);
            this._doReset();
        }, process: function (dataUpdate) {
            this._append(dataUpdate);
            return this._process();
        }, finalize: function (dataUpdate) {
            if (dataUpdate) {
                this._append(dataUpdate);
            }
            var finalProcessedData = this._doFinalize();
            return finalProcessedData;
        }, keySize: 128 / 32,
        ivSize: 128 / 32,
        _ENC_XFORM_MODE: 1,
        _DEC_XFORM_MODE: 2,
        _createHelper: (function () {
            function selectCipherStrategy(key) {
                if (typeof key == 'string') {
                    return PasswordBasedCipher;
                } else {
                    return SerializableCipher;
                }
            }
            return function (cipher) {
                return {
                    encrypt: function (message, key, cfg) {
                        return selectCipherStrategy(key).encrypt(cipher, message, key, cfg);
                    }, decrypt: function (ciphertext, key, cfg) {
                        return selectCipherStrategy(key).decrypt(cipher, ciphertext, key, cfg);
                    }
                };
            };
        }())
    });
    var StreamCipher = C_lib.StreamCipher = Cipher.extend({
        _doFinalize: function () {
            var finalProcessedBlocks = this._process(!!'flush');
            return finalProcessedBlocks;
        }, blockSize: 1
    });
    var C_mode = C.mode = {};
    var BlockCipherMode = C_lib.BlockCipherMode = Base.extend({
        createEncryptor: function (cipher, iv) {
            return this.Encryptor.create(cipher, iv);
        }, createDecryptor: function (cipher, iv) {
            return this.Decryptor.create(cipher, iv);
        }, init: function (cipher, iv) {
            this._cipher = cipher;
            this._iv = iv;
        }
    });
    var CBC = C_mode.CBC = (function () {
        var CBC = BlockCipherMode.extend();
        CBC.Encryptor = CBC.extend({
            processBlock: function (words, offset) {
                var cipher = this._cipher;
                var blockSize = cipher.blockSize;
                xorBlock.call(this, words, offset, blockSize);
                cipher.encryptBlock(words, offset);
                this._prevBlock = words.slice(offset, offset + blockSize);
            }
        });
        CBC.Decryptor = CBC.extend({
            processBlock: function (words, offset) {
                var cipher = this._cipher;
                var blockSize = cipher.blockSize;
                var thisBlock = words.slice(offset, offset + blockSize);
                cipher.decryptBlock(words, offset);
                xorBlock.call(this, words, offset, blockSize);
                this._prevBlock = thisBlock;
            }
        });

        function xorBlock(words, offset, blockSize) {
            var iv = this._iv;
            if (iv) {
                var block = iv;
                this._iv = undefined;
            } else {
                var block = this._prevBlock;
            }
            for (var i = 0; i < blockSize; i++) {
                words[offset + i] ^= block[i];
            }
        }
        return CBC;
    }());
    var C_pad = C.pad = {};
    var Pkcs7 = C_pad.Pkcs7 = {
        pad: function (data, blockSize) {
            var blockSizeBytes = blockSize * 4;
            var nPaddingBytes = blockSizeBytes - data.sigBytes % blockSizeBytes;
            var paddingWord = (nPaddingBytes << 24) | (nPaddingBytes << 16) | (nPaddingBytes << 8) | nPaddingBytes;
            var paddingWords = [];
            for (var i = 0; i < nPaddingBytes; i += 4) {
                paddingWords.push(paddingWord);
            }
            var padding = WordArray.create(paddingWords, nPaddingBytes);
            data.concat(padding);
        }, unpad: function (data) {
            var nPaddingBytes = data.words[(data.sigBytes - 1) >>> 2] & 0xff;
            data.sigBytes -= nPaddingBytes;
        }
    };
    var BlockCipher = C_lib.BlockCipher = Cipher.extend({
        cfg: Cipher.cfg.extend({
            mode: CBC,
            padding: Pkcs7
        }),
        reset: function () {
            Cipher.reset.call(this);
            var cfg = this.cfg;
            var iv = cfg.iv;
            var mode = cfg.mode;
            if (this._xformMode == this._ENC_XFORM_MODE) {
                var modeCreator = mode.createEncryptor;
            } else {
                var modeCreator = mode.createDecryptor;
                this._minBufferSize = 1;
            } if (this._mode && this._mode.__creator == modeCreator) {
                this._mode.init(this, iv && iv.words);
            } else {
                this._mode = modeCreator.call(mode, this, iv && iv.words);
                this._mode.__creator = modeCreator;
            }
        }, _doProcessBlock: function (words, offset) {
            this._mode.processBlock(words, offset);
        }, _doFinalize: function () {
            var padding = this.cfg.padding;
            if (this._xformMode == this._ENC_XFORM_MODE) {
                padding.pad(this._data, this.blockSize);
                var finalProcessedBlocks = this._process(!!'flush');
            } else {
                var finalProcessedBlocks = this._process(!!'flush');
                padding.unpad(finalProcessedBlocks);
            }
            return finalProcessedBlocks;
        }, blockSize: 128 / 32
    });
    var CipherParams = C_lib.CipherParams = Base.extend({
        init: function (cipherParams) {
            this.mixIn(cipherParams);
        }, toString: function (formatter) {
            return (formatter || this.formatter).stringify(this);
        }
    });
    var C_format = C.format = {};
    var OpenSSLFormatter = C_format.OpenSSL = {
        stringify: function (cipherParams) {
            var ciphertext = cipherParams.ciphertext;
            var salt = cipherParams.salt;
            if (salt) {
                var wordArray = WordArray.create([0x53616c74, 0x65645f5f]).concat(salt).concat(ciphertext);
            } else {
                var wordArray = ciphertext;
            }
            return wordArray.toString(Base64);
        }, parse: function (openSSLStr) {
            var ciphertext = Base64.parse(openSSLStr);
            var ciphertextWords = ciphertext.words;
            if (ciphertextWords[0] == 0x53616c74 && ciphertextWords[1] == 0x65645f5f) {
                var salt = WordArray.create(ciphertextWords.slice(2, 4));
                ciphertextWords.splice(0, 4);
                ciphertext.sigBytes -= 16;
            }
            return CipherParams.create({
                ciphertext: ciphertext,
                salt: salt
            });
        }
    };
    var SerializableCipher = C_lib.SerializableCipher = Base.extend({
        cfg: Base.extend({
            format: OpenSSLFormatter
        }),
        encrypt: function (cipher, message, key, cfg) {
            cfg = this.cfg.extend(cfg);
            var encryptor = cipher.createEncryptor(key, cfg);
            var ciphertext = encryptor.finalize(message);
            var cipherCfg = encryptor.cfg;
            return CipherParams.create({
                ciphertext: ciphertext,
                key: key,
                iv: cipherCfg.iv,
                algorithm: cipher,
                mode: cipherCfg.mode,
                padding: cipherCfg.padding,
                blockSize: cipher.blockSize,
                formatter: cfg.format
            });
        }, decrypt: function (cipher, ciphertext, key, cfg) {
            cfg = this.cfg.extend(cfg);
            ciphertext = this._parse(ciphertext, cfg.format);
            var plaintext = cipher.createDecryptor(key, cfg).finalize(ciphertext.ciphertext);
            return plaintext;
        }, _parse: function (ciphertext, format) {
            if (typeof ciphertext == 'string') {
                return format.parse(ciphertext, this);
            } else {
                return ciphertext;
            }
        }
    });
    var C_kdf = C.kdf = {};
    var OpenSSLKdf = C_kdf.OpenSSL = {
        execute: function (password, keySize, ivSize, salt) {
            if (!salt) {
                salt = WordArray.random(64 / 8);
            }
            var key = EvpKDF.create({
                keySize: keySize + ivSize
            }).compute(password, salt);
            var iv = WordArray.create(key.words.slice(keySize), ivSize * 4);
            key.sigBytes = keySize * 4;
            return CipherParams.create({
                key: key,
                iv: iv,
                salt: salt
            });
        }
    };
    var PasswordBasedCipher = C_lib.PasswordBasedCipher = SerializableCipher.extend({
        cfg: SerializableCipher.cfg.extend({
            kdf: OpenSSLKdf
        }),
        encrypt: function (cipher, message, password, cfg) {
            cfg = this.cfg.extend(cfg);
            var derivedParams = cfg.kdf.execute(password, cipher.keySize, cipher.ivSize);
            cfg.iv = derivedParams.iv;
            var ciphertext = SerializableCipher.encrypt.call(this, cipher, message, derivedParams.key, cfg);
            ciphertext.mixIn(derivedParams);
            return ciphertext;
        }, decrypt: function (cipher, ciphertext, password, cfg) {
            cfg = this.cfg.extend(cfg);
            ciphertext = this._parse(ciphertext, cfg.format);
            var derivedParams = cfg.kdf.execute(password, cipher.keySize, cipher.ivSize, ciphertext.salt);
            cfg.iv = derivedParams.iv;
            var plaintext = SerializableCipher.decrypt.call(this, cipher, ciphertext, derivedParams.key, cfg);
            return plaintext;
        }
    });
}());

(function () {
    var C = CryptoJS;
    var C_lib = C.lib;
    var BlockCipher = C_lib.BlockCipher;
    var C_algo = C.algo;
    var SBOX = [];
    var INV_SBOX = [];
    var SUB_MIX_0 = [];
    var SUB_MIX_1 = [];
    var SUB_MIX_2 = [];
    var SUB_MIX_3 = [];
    var INV_SUB_MIX_0 = [];
    var INV_SUB_MIX_1 = [];
    var INV_SUB_MIX_2 = [];
    var INV_SUB_MIX_3 = [];
    (function () {
        var d = [];
        for (var i = 0; i < 256; i++) {
            if (i < 128) {
                d[i] = i << 1;
            } else {
                d[i] = (i << 1) ^ 0x11b;
            }
        }
        var x = 0;
        var xi = 0;
        for (var i = 0; i < 256; i++) {
            var sx = xi ^ (xi << 1) ^ (xi << 2) ^ (xi << 3) ^ (xi << 4);
            sx = (sx >>> 8) ^ (sx & 0xff) ^ 0x63;
            SBOX[x] = sx;
            INV_SBOX[sx] = x;
            var x2 = d[x];
            var x4 = d[x2];
            var x8 = d[x4];
            var t = (d[sx] * 0x101) ^ (sx * 0x1010100);
            SUB_MIX_0[x] = (t << 24) | (t >>> 8);
            SUB_MIX_1[x] = (t << 16) | (t >>> 16);
            SUB_MIX_2[x] = (t << 8) | (t >>> 24);
            SUB_MIX_3[x] = t;
            var t = (x8 * 0x1010101) ^ (x4 * 0x10001) ^ (x2 * 0x101) ^ (x * 0x1010100);
            INV_SUB_MIX_0[sx] = (t << 24) | (t >>> 8);
            INV_SUB_MIX_1[sx] = (t << 16) | (t >>> 16);
            INV_SUB_MIX_2[sx] = (t << 8) | (t >>> 24);
            INV_SUB_MIX_3[sx] = t;
            if (!x) {
                x = xi = 1;
            } else {
                x = x2 ^ d[d[d[x8 ^ x2]]];
                xi ^= d[d[xi]];
            }
        }
    }());
    var RCON = [0x00, 0x01, 0x02, 0x04, 0x08, 0x10, 0x20, 0x40, 0x80, 0x1b, 0x36];
    var AES = C_algo.AES = BlockCipher.extend({
        _doReset: function () {
            if (this._nRounds && this._keyPriorReset === this._key) {
                return;
            }
            var key = this._keyPriorReset = this._key;
            var keyWords = key.words;
            var keySize = key.sigBytes / 4;
            var nRounds = this._nRounds = keySize + 6;
            var ksRows = (nRounds + 1) * 4;
            var keySchedule = this._keySchedule = [];
            for (var ksRow = 0; ksRow < ksRows; ksRow++) {
                if (ksRow < keySize) {
                    keySchedule[ksRow] = keyWords[ksRow];
                } else {
                    var t = keySchedule[ksRow - 1];
                    if (!(ksRow % keySize)) {
                        t = (t << 8) | (t >>> 24);
                        t = (SBOX[t >>> 24] << 24) | (SBOX[(t >>> 16) & 0xff] << 16) | (SBOX[(t >>> 8) & 0xff] << 8) | SBOX[t & 0xff];
                        t ^= RCON[(ksRow / keySize) | 0] << 24;
                    } else if (keySize > 6 && ksRow % keySize == 4) {
                        t = (SBOX[t >>> 24] << 24) | (SBOX[(t >>> 16) & 0xff] << 16) | (SBOX[(t >>> 8) & 0xff] << 8) | SBOX[t & 0xff];
                    }
                    keySchedule[ksRow] = keySchedule[ksRow - keySize] ^ t;
                }
            }
            var invKeySchedule = this._invKeySchedule = [];
            for (var invKsRow = 0; invKsRow < ksRows; invKsRow++) {
                var ksRow = ksRows - invKsRow;
                if (invKsRow % 4) {
                    var t = keySchedule[ksRow];
                } else {
                    var t = keySchedule[ksRow - 4];
                } if (invKsRow < 4 || ksRow <= 4) {
                    invKeySchedule[invKsRow] = t;
                } else {
                    invKeySchedule[invKsRow] = INV_SUB_MIX_0[SBOX[t >>> 24]] ^ INV_SUB_MIX_1[SBOX[(t >>> 16) & 0xff]] ^ INV_SUB_MIX_2[SBOX[(t >>> 8) & 0xff]] ^ INV_SUB_MIX_3[SBOX[t & 0xff]];
                }
            }
        }, encryptBlock: function (M, offset) {
            this._doCryptBlock(M, offset, this._keySchedule, SUB_MIX_0, SUB_MIX_1, SUB_MIX_2, SUB_MIX_3, SBOX);
        }, decryptBlock: function (M, offset) {
            var t = M[offset + 1];
            M[offset + 1] = M[offset + 3];
            M[offset + 3] = t;
            this._doCryptBlock(M, offset, this._invKeySchedule, INV_SUB_MIX_0, INV_SUB_MIX_1, INV_SUB_MIX_2, INV_SUB_MIX_3, INV_SBOX);
            var t = M[offset + 1];
            M[offset + 1] = M[offset + 3];
            M[offset + 3] = t;
        }, _doCryptBlock: function (M, offset, keySchedule, SUB_MIX_0, SUB_MIX_1, SUB_MIX_2, SUB_MIX_3, SBOX) {
            var nRounds = this._nRounds;
            var s0 = M[offset] ^ keySchedule[0];
            var s1 = M[offset + 1] ^ keySchedule[1];
            var s2 = M[offset + 2] ^ keySchedule[2];
            var s3 = M[offset + 3] ^ keySchedule[3];
            var ksRow = 4;
            for (var round = 1; round < nRounds; round++) {
                var t0 = SUB_MIX_0[s0 >>> 24] ^ SUB_MIX_1[(s1 >>> 16) & 0xff] ^ SUB_MIX_2[(s2 >>> 8) & 0xff] ^ SUB_MIX_3[s3 & 0xff] ^ keySchedule[ksRow++];
                var t1 = SUB_MIX_0[s1 >>> 24] ^ SUB_MIX_1[(s2 >>> 16) & 0xff] ^ SUB_MIX_2[(s3 >>> 8) & 0xff] ^ SUB_MIX_3[s0 & 0xff] ^ keySchedule[ksRow++];
                var t2 = SUB_MIX_0[s2 >>> 24] ^ SUB_MIX_1[(s3 >>> 16) & 0xff] ^ SUB_MIX_2[(s0 >>> 8) & 0xff] ^ SUB_MIX_3[s1 & 0xff] ^ keySchedule[ksRow++];
                var t3 = SUB_MIX_0[s3 >>> 24] ^ SUB_MIX_1[(s0 >>> 16) & 0xff] ^ SUB_MIX_2[(s1 >>> 8) & 0xff] ^ SUB_MIX_3[s2 & 0xff] ^ keySchedule[ksRow++];
                s0 = t0;
                s1 = t1;
                s2 = t2;
                s3 = t3;
            }
            var t0 = ((SBOX[s0 >>> 24] << 24) | (SBOX[(s1 >>> 16) & 0xff] << 16) | (SBOX[(s2 >>> 8) & 0xff] << 8) | SBOX[s3 & 0xff]) ^ keySchedule[ksRow++];
            var t1 = ((SBOX[s1 >>> 24] << 24) | (SBOX[(s2 >>> 16) & 0xff] << 16) | (SBOX[(s3 >>> 8) & 0xff] << 8) | SBOX[s0 & 0xff]) ^ keySchedule[ksRow++];
            var t2 = ((SBOX[s2 >>> 24] << 24) | (SBOX[(s3 >>> 16) & 0xff] << 16) | (SBOX[(s0 >>> 8) & 0xff] << 8) | SBOX[s1 & 0xff]) ^ keySchedule[ksRow++];
            var t3 = ((SBOX[s3 >>> 24] << 24) | (SBOX[(s0 >>> 16) & 0xff] << 16) | (SBOX[(s1 >>> 8) & 0xff] << 8) | SBOX[s2 & 0xff]) ^ keySchedule[ksRow++];
            M[offset] = t0;
            M[offset + 1] = t1;
            M[offset + 2] = t2;
            M[offset + 3] = t3;
        }, keySize: 256 / 32
    });
    C.AES = BlockCipher._createHelper(AES);
}());

var key = CryptoJS.enc.Hex.parse("4587dc9b6a7c3e9ef3b920f994edc3a210c460977528138d41e58b9b02c94ffd");
var iv = CryptoJS.enc.Hex.parse("6aa677d0f4d6646eec5e9a82aedb60b0");

function AES_Encrypt(word) {
    var srcs = CryptoJS.enc.Utf8.parse(word);
    var encrypted = CryptoJS.AES.encrypt(srcs, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    return CryptoJS.enc.Hex.stringify(CryptoJS.enc.Base64.parse(encrypted.toString()));
}

function AES_Decrypt(word) {
    var srcs = CryptoJS.enc.Base64.stringify(CryptoJS.enc.Hex.parse(word));
    var decrypt = CryptoJS.AES.decrypt(srcs, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    return decrypt.toString(CryptoJS.enc.Utf8);
}

function get_data(data){
    return AES_Decrypt(data)
}
```

[](http://49.234.55.187:8090/archives/mou-gao-kao-ping-tai-ni-xiang)
