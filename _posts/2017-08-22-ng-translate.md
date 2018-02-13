---
layout: post
title: Angular国际化方案(ngx-translate)
date: 2017-08-22
tags: [JavaScript]
---
* auto-gen TOC:
{:toc}

本文只针对ngx-translate/core 6.x版本，如果你使用的是5.x或者更低的版本，请参照以下链接。

[https://github.com/ngx-translate/core/blob/fb02ca5920aae405048ebab50e09db67d5bf12a2/README.md](https://github.com/ngx-translate/core/blob/fb02ca5920aae405048ebab50e09db67d5bf12a2/README.md)


### 安装

首先需要安装npm依赖：
```shell
npm install @ngx-translate/core --save
npm install @ngx-translate/http-loader --save // 针对Angular>=4.3
npm install @ngx-translate/http-loader@0.1.0 --save // 针对Angular<4.3
```
这里需要注意，如果你使用的Angular版本是 **Angular < 4.3**，那么需要安装http-loader@0.1.0版本。

因为0.1.0以后的版本TranslateHttpLoader构造函数的第一个参数改为HttpClient类型，而非Http类型。


### 用法

#### 1、引入TranslateModule模块

首先需要在你项目的root NgModule中引入TranslateModule.forRoot()模块。一般在项目中默认命名为app.module.ts。

```javascript
import {NgModule} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {HttpClientModule, HttpClient} from '@angular/common/http';
import {TranslateModule, TranslateLoader} from '@ngx-translate/core';
import {TranslateHttpLoader} from '@ngx-translate/http-loader';
import {AppComponent} from './app';
export function createTranslateLoader(http: HttpClient) {
    return new TranslateHttpLoader(http, './assets/i18n/', '.json');
}

@NgModule({
    imports: [
        BrowserModule,
        HttpClientModule,
        TranslateModule.forRoot({
            loader: {
                provide: TranslateLoader,
                useFactory: (createTranslateLoader),
                deps: [HttpClient]
            }
        })
    ],
    bootstrap: [AppComponent]
})
export class AppModule { }
```
这里使用了TranslateHttpLoader 来加载我们定义好的语言文件。"/assets/i18n/[lang].json"这里的lang就是当前正在使用的语言。

注意：如果当前采用的是AOT编译方式或者是ionic2工程，那么useFactory对应的必须是一个export的自定义方法而非内联方法。

 即以下这种方式是不被允许的：

```javascript
@NgModule({
    imports: [
        BrowserModule,
        HttpModule,
        TranslateModule.forRoot({
            provide: TranslateLoader,
            useFactory: (http: HttpClient) => new TranslateStaticLoader(http, '/assets/i18n', '.json'),
            deps: [HttpClient]
        })
    ],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

#### 2、注入TranslateService 服务

在需要用到的component里面注入TranslateService。

```javascript
import {TranslateService} from '@ngx-translate/core';
```
然后在构造函数中定义当前应用的默认语言。

```javascript
constructor(private translate: TranslateService) {
  // this language will be used as a fallback when a translation isn't found in the current language
  translate.setDefaultLang('en');

  // use the brower's default lang, if the lang isn't available, it will use the 'en'
  let broswerLang = translate.getBrowserLang();
  translate.use(broswerLang.match(/en|cn/) ? broswerLang : 'en');
}
```

#### 3、翻译文本书写规则

有两种方式可以加载我们翻译好的语言文本。

首先你可以把翻译好的语言文本放到一个json文件中，然后通过TranslateHttpLoader来引用这个json文件。

例如：en.json

```javascript
{
    "HELLO": "hello {{value}}"
}
```
另外也可以通过setTranslation方法手动加载。

```javascript
translate.setTranslation('en', {
    HELLO: 'hello {{value}}'
});
```
同时，这里的json结构是支持嵌套的。

```javascript
{
    "HOME": {
        "HELLO": "hello {{value}}"
    }
}
```
以上结构，可以通过"HOME.HELLO"来引用HELLO的内容。


#### 4、使用方法

我们可以通过TranslateService, TranslatePipe 或者 TranslateDirective这三种方式来获取我们翻译的文本内容。

TranslateService：

```javascript
translate.get('HELLO', {value: 'world'}).subscribe((res: string) => {
    console.log(res);
    //=> 'hello world'
});
```
 其中第二个参数{value: 'world'}是可选的。

TranslateService：

```javascript
<div>{{ 'HELLO' | translate:param }}</div>
```
param可以像如下方式在component里面定义。同样，这个参数也是可选的。

```javascript
param = {value: 'world'};
```

 TranslateDirective：

```javascript
<div [translate]="'HELLO'" [translateParams]="{value: 'world'}"></div>
```
或者

```javascript
<div translate [translateParams]="{value: 'world'}">HELLO</div>
```


#### 5、使用HTML标签

允许在你的翻译文本中直接嵌入HTML标签。

```javascript
{
    "HELLO": "Welcome to my Angular application!<br><strong>This is an amazing app which uses the latest technologies!</strong>"
}
```
这时可以使用innerHTML 来进行渲染。

```javascript
<div [innerHTML]="'HELLO' | translate"></div>
```

TranslateService API
公有属性(public properties):

    /**
     * The default lang to fallback when translations are missing on the current lang
     */
    defaultLang: string;
    /**
     * The lang currently used
     * @type {string}
     */
    currentLang: string;
    /**
     * an array of langs
     * @type {Array}
     */
    langs: string[];
    /**
     * a list of translations per lang
     * @type {{}}
     */
    translations: any;

公有方法(public methods):

```javascript
/**
 * Sets the default language to use as a fallback
 * @param lang
 */
setDefaultLang(lang: string): void;
/**
 * Gets the default language used
 * @returns string
 */
getDefaultLang(): string;
/**
 * Changes the lang currently used
 * @param lang
 * @returns {Observable<*>}
 */
use(lang: string): Observable<any>;
/**
 * Gets an object of translations for a given language with the current loader
 * and passes it through the compiler
 * @param lang
 * @returns {Observable<*>}
 */
getTranslation(lang: string): Observable<any>;
/**
 * Manually sets an object of translations for a given language
 * after passing it through the compiler
 * @param lang
 * @param translations
 * @param shouldMerge
 */
setTranslation(lang: string, translations: Object, shouldMerge?: boolean): void;
/**
 * Returns an array of currently available langs
 * @returns {any}
 */
getLangs(): Array<string>;
/**
 * @param langs
 * Add available langs
 */
addLangs(langs: Array<string>): void;
/**
 * Returns the parsed result of the translations
 * @param translations
 * @param key
 * @param interpolateParams
 * @returns {any}
 */
getParsedResult(translations: any, key: any, interpolateParams?: Object): any;
/**
 * Gets the translated value of a key (or an array of keys)
 * @param key
 * @param interpolateParams
 * @returns {any} the translated key, or an object of translated keys
 */
get(key: string | Array<string>, interpolateParams?: Object): Observable<string | any>;
/**
 * Returns a stream of translated values of a key (or an array of keys) which updates
 * whenever the language changes.
 * @param key
 * @param interpolateParams
 * @returns {any} A stream of the translated key, or an object of translated keys
 */
stream(key: string | Array<string>, interpolateParams?: Object): Observable<string | any>;
/**
 * Returns a translation instantly from the internal state of loaded translation.
 * All rules regarding the current language, the preferred language of even fallback languages will be used except any promise handling.
 * @param key
 * @param interpolateParams
 * @returns {string}
 */
instant(key: string | Array<string>, interpolateParams?: Object): string | any;
/**
 * Sets the translated value of a key, after compiling it
 * @param key
 * @param value
 * @param lang
 */
set(key: string, value: string, lang?: string): void;
/**
 * Allows to reload the lang file from the file
 * @param lang
 * @returns {Observable<any>}
 */
reloadLang(lang: string): Observable<any>;
/**
 * Deletes inner translation
 * @param lang
 */
resetLang(lang: string): void;
/**
 * Returns the language code name from the browser, e.g. "de"
 *
 * @returns string
 */
getBrowserLang(): string;
/**
 * Returns the culture language code name from the browser, e.g. "de-DE"
 *
 * @returns string
 */
getBrowserCultureLang(): string;
```