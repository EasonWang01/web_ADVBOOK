
# #Material UI

類似於Bootstrap，為google Material Design 的網頁實作UI框架 https://material.io/guidelines/#


我們這次會使用Material Design 的React.js實作版本Material UI
當我們的前端UI框架，所以我們先到他的網站看其原件的使用方法
http://www.material-ui.com/#/


使用它包好的元件

例如:

```
import RaisedButton from 'material-ui/RaisedButton';

<RaisedButton label="Primary" primary={true} style={style} />
```

我們到http://www.material-ui.com/#/components/raised-button

可直接點選`<>`橫條部分即會出現程式碼範例，拉到頁面最下方會顯示該元件可使用的所有`props`

#第一部分

先從單元上點選按鈕下載這次的課程範例

這一部分講解有關以下三個部分

```
1.React Server Side Rendering

2.註冊

3.登入

```

我們把下載下來的檔案解壓縮後即可打開，之後一樣用`terminal`之後`cd`到該目錄輸入`npm install`，大約需等候1~3分鐘

##1.React Server Side Rendering

簡介：我們寫React.js時轉換頁面都不會重新跟server要資料，所以才會不需要進行重新整理，但這樣子我們頁面都是由js所產生的，而老舊的搜尋引擎比較不容易找到此種類型的網站，所以我們可以讓React使用Server Side Rendering，讓第一次讀取到頁面時也從資料庫加載一些我們所需要的資料進來


```javascript
import {renderToString} from 'react-dom/server';
import {RouterContext, match, createRoutes} from 'react-router';


app.get('*', (req, res) => {

	let initialState = {
			todos:[{
				id:0,
				completed: false,
				text:'initial for demo'
			}],
			userInfo:{
				
			}
	}
	const store = configureStore(initialState);
	const muiTheme = getMuiTheme({
	  userAgent: req.headers['user-agent'],
	});
  match({routes, location: req.url}, (error, redirectLocation, renderProps) => {
    if (error) {
      res.status(500).send(error.message);
    } else if (redirectLocation) {
      res.redirect(302, redirectLocation.pathname + redirectLocation.search);
    } else if (renderProps) {
      const content = renderToString(
				<Provider store={store}>
				  <MuiThemeProvider muiTheme={muiTheme}>
					  <RouterContext {...renderProps} />
				  </MuiThemeProvider>
				</Provider>
			);
      let state = store.getState();
      let page = renderFullPage(content, state);
      return res.status(200).send(page);
    } else {
      res.status(404).send('Not Found');
    }
  });
});
```

```html
const renderFullPage = (html, preloadedState) => (`
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=0, maximum-scale=1, minimum-scale=1">
  <title>React Todo List</title>
	<link rel="stylesheet" type="text/css" href="/css/reset.css">

</head>
<body>
  <div id="app">${html}</div>
  <script>
  window.__PRELOADED_STATE__ = ${JSON.stringify(preloadedState).replace(/</g, '\\x3c')}
   </script>
  <script src="vendor.bundle.js"></script>
  <script src="bundle.js"></script>
</body>
</html>
`
);

```


包含上面和之後所看到有關

```
MuiTheme
```

都是Material UI中的環境配置需求


接著是Router

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { render } from 'react-dom'
import root from './root.js'
import {Provider} from 'react-redux'
import {configureStore} from '../redux/store'
import {Router, browserHistory, Route} from 'react-router';
import MuiThemeProvider from 'material-ui/styles/MuiThemeProvider';
import injectTapEventPlugin from 'react-tap-event-plugin';
const initialState = window.__PRELOADED_STATE__;
injectTapEventPlugin();//Material UI中用來解決onClick相關Bug
const store = configureStore(initialState);

ReactDOM.render(
	<Provider store={store}>
		<MuiThemeProvider>
	    <Router history={browserHistory} routes={root} />
		</MuiThemeProvider>
	</Provider>
,document.getElementById('app')
)

```