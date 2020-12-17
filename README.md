# Eyeofcloud WechatMini SDK

This repository houses the WechatMini SDK for use with Eyeofcloud Full Stack.

Eyeofcloud Full Stack is A/B testing and feature flag management for product development teams.


## Getting Started


### Requirements
*  Wechat that support Wechat mini program


### Installing the SDK
To add the wechat mini sdk to your project, download sdk and put it in any directory, include it with 'require' method.



### Samples
A sample code for SDK initialization and experiments:

1. Reference sdk file in 'eoc_manager.js'
```
var eyeofcloud = require('eyeofcloud.min.js');
const PROJECT_URL = 'https://cdn.eyeofcloud.com/portal/editor/config/json/fullstack/8_3e9f2898e924f4961afb27b57ee7da08.json'
var eocInstance;
var datafile;

module.exports = {  
  getInstance(cb) {    //Get Eyeofcloud Instance
    return new Promise((resolve, rejetc) => {
      if (!datafile || cb) {
        getDatafile()
          .then((fetchedDatafile) => {
            datafile = fetchedDatafile;
            var instance = _getInstance(fetchedDatafile);
            resolve(instance)
          })
      } else {
        var instance = _getInstance(datafile);
        resolve(instance)
      }
    })
  }
}

function _getInstance  (datafile) {
  if (!eocInstance) {
    eocInstance = eyeofcloud.createInstance({
      datafile,
      skipJSONvalidation: true,
    })
  //For Eyeofcloud private deployment, URL should be set for message receiving;     for Eyeofcloud Cloudplatform deployment(SaaS), the following code could be commented out
  //eyeofcloud.setServerHost("https://www.mysite.com/eyeofcloud/tracker");
  }
  return eocInstance
}

function getDatafile () {
  return new Promise((resolve, reject) => {
    wx.request({
      url: PROJECT_URL,
      success: function (res) {
        console.log(res.data)
        resolve(res.data)
      }
    })
  })
}
```

2. Reference 'eoc_manager.js' in other file
```
var eocmanager = require('./eoc_manager.js')
onLoad: function(){
	var that = this;
	app.getOpenId(function(openId){
		eocmanager.getInstance().then(function(eyeofcloud){		
		//Activate experiment
		var variation = eyeofcloud.activate('experiment',openId);
		if('variationA' == variation)
			setData({text:'variationA'})
		else if('variationB' == variation)
			setData({text:'variationB'})
		})
	})
}
```

### Further References

1. User Manuals: 
https://www.eyeofcloud.com/user_help#tab-%E7%A7%BB%E5%8A%A8%E7%AB%AFab%E6%B5%8B%E8%AF%95

2. Development Guidance: 
https://www.eyeofcloud.com/help/x/solutions/sdks/introduction/index.html?language=wx&platform=mobile



