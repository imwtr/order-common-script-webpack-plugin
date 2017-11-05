[![npm][npm]][npm-url]
# Description
A plugin for html-webpack-plugin, reorder common chunkFiles beyond `<script>` tags to make sure jQuery plugins work

When using jQuery plugins in webpack and the jQuery itself is expose(`expose-loader`) by webpack, then we put it into the common chunks, now using `html-webpack-plugin` to compile this template file
``` html
    <script src="/public/static/libs/abc/jquery.abc.js"></script>
    <script src="http://localhost:8188/dist/js/common.js"></script>
    <script src="http://localhost:8188/dist/js/home.js"></script>
</body>
</html>
```
And we got
```  html
    <script src=/public/static/libs/abc/jquery.abc.js></script>
    <script src=http://localhost:8188/dist/js/common.js></script>
    <script src=http://localhost:8188/dist/js/home.js></script>
    <script type="text/javascript" src="/public/static/dist/js/common.js?e0697ddd"></script>
    <script type="text/javascript" src="/public/static/dist/js/home.js?f0f6b1f2"></script>
</body>
</html>
```
The abc jQuery plugin won't works, we need to reorder the  `common.js` file before the `jquery.abc.js`



# Arguments
## `Options([Object = {commonName: 'common'}])` 
Pass options to the plugin

`commonName: String|Arrray(String)`
Config the common chunks, default is common, also works with mutiple chunks


# Installation
Using npm
```javascript
npm install order-common-script-webpack-plugin --save-dev
```


# Usage
Config your common chunks
```javascript
//  webpack.config.js

let HtmlOrderCommonScriptPlugin = require('order-common-script-webpack-plugin');

// ...

module.exports = {

    // ...
    
    entry: {
        home: './src/js/home',
        detail: './src/js/detail',
        // config you common chunks
        // vendor: ['react', 'react-dom', 'jquery']
        common: ['jquery']
    },

    plugins: [

        // ...

        new webpack.optimize.CommonsChunkPlugin({
            chunks: ['home', 'detail'],
            filename: '[name].js',
            // config your common chunks
            // names: ['manifest', 'vendor']
            name: 'common'
        }),

        new HtmlWebpackPlugin({
            template: '../../views/detail/detail_tpl.html',
            filename: '../../../../views/detail/detail.html',
            // config your common chunks
            // chunks: ['manifest', 'vendor', 'detail']
            chunks: ['common', 'detail'],
            inject: true
        }),
        new HtmlOrderCommonScriptPlugin({
            // config your commonChunkName, default:common
            //commonName: ['manifest', 'vendor']
        })
    ]
}
```


