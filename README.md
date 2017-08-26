# \<nylon-pages\>

`nylon-pages` is an router element for polymer.
Use page.js dependency 

Here is a basic example.
```html
    <nylon-pages>
        <div path="/">Hello index.</div>
        <div path="/home">Hello home.</div>
        <div path="*">Sorry 404 T_T</div>
    </nylon-pages>
```

Here is a basic example, when used with `another element`.
```html
    <nylon-pages>
        <page-index path="/"></page-index>
        <page-home path="/home"></page-home>
        <page-404 path="*"></page-404>
    </nylon-pages>
```

Here is a basic example, when used with `lazy load`.
```html
    <nylon-pages on-nylon-pages-changed="_pageChanged">
        <page-index path="/"></page-index>
        <page-home path="/home"></page-home>
        <page-404 path="*"></page-404>
    </nylon-pages>
```
```js
    _pageChanged(e){
        var elementName = e.detail.element.localName;
        this.importHref(
            this.resolveUrl('/src/' + elementName + '.html'),
            null,
            null,
            true
        );
    }
```

How to get params on `children element`.
```html
    <nylon-pages on-nylon-pages-changed="_pageChanged">
        <page-index path="/"></page-index>
        <page-home path="/home"></page-home>
        <page-topic path="/topic/:id"></page-topic>
        <page-404 path="*"></page-404>
    </nylon-pages>
```
```html
    <!-- children element-->
    <template>
        [[_params.id]]
    </template>
```

How to change page on `polymer`.
```html
    <button on-click="simpleFunc">change</button>
    <a href="/topic/1">link</a>
    <nylon-pages id="router" on-nylon-pages-changed="_pageChanged">
        <page-index path="/"></page-index>
        <page-home path="/home"></page-home>
        <page-topic path="/topic/:id"></page-topic>
        <page-404 path="*"></page-404>
    </nylon-pages>
```
```js
    simpleFunc(){
        this.$.router.redirect('/topic/1')
    }
```

How to check role and redirect page.
```html
    <nylon-pages on-nylon-pages-role="_checkRole">
        <page-index path="/"></page-index>
        <page-home path="/home"></page-home>
        <page-topic path="/topic/:id" role></page-topic>
        <page-topic-select path="/user/select/:id" role="admin"></page-topic-select>
        <page-401 path="/401"></page-404>
        <page-403 path="/403"></page-403>
        <page-404 path="*"></page-404>
    </nylon-pages>
```
```js
    _checkRole(e){
        var np = e.detail

        var element = np.element
        var elementName = element.localName;
        var roleValue = element.getAttribute('role')

        var pass = await _checkRoleAnotherFun(...)

        if(pass){
            np.pass()
        }else{
            np.redirect('/403')
        }
    }
```

How to check role and redirect page. but `no change path`
```html
    <nylon-pages on-nylon-pages-role="_checkRole">
        <page-index path="/"></page-index>
        <page-home path="/home"></page-home>
        <page-topic path="/topic/:id" role></page-topic>
        <page-topic-select path="/user/select/:id" role="admin"></page-topic-select>
        <page-401 path="*"></page-404>
        <page-403 path="*"></page-403>
        <page-404 path="*"></page-404>
    </nylon-pages>
```
```js
    _checkRole(e){
        var np = e.detail

        var element = np.element
        var elementName = element.localName;
        var roleValue = element.getAttribute('role')

        if(np.ctx.gotoPage){
            if(elementName=='page-403' && np.ctx.gotoPage=='403')
                np.pass()
        }else{
            var pass = await _checkRoleAnotherFun(...)
            if(pass){
                np.pass()
            }else{
                np.ctx.gotoPage = "403"
            }
        }
    }
```
