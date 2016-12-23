# qq-connect - ThinkPHP 5 QQ登录

用于tp5框架下的qq快捷登录

composer安装: composer require sunnnnn/qq-connect

添加公共配置:
config.php
// QQ 互联配置
'qqconnect' => [
    'appid' => '',
    'appkey' => '',
    'callback' => '',
    'scope' => 'get_user_info,add_share,list_album,add_album,upload_pic,add_topic,add_one_blog,add_weibo,check_page_fans,add_t,add_pic_t,del_t,get_repost_list,get_info,get_other_info,get_fanslist,get_idolist,add_idol,del_idol,get_tenpay_addr',
    'errorReport' => true
]
```

## 示例代码

### 页面编写:
```
<a href="{:url('/oauth/qqLogin')}">QQ登录</a>
```

### 控制器编写:

登录
Oauth.php
use sunnnnn\qqconnect\QC;
class OauthController extends \think\Controller
{
    public function qqLogin()
    {
        $qc = new QC();
        return $this->redirect($qc->qq_login());
    }
}

回调
Callback.php
use sunnnnn\qqconnect\QC;
class CallbackController extends \think\Controller
{
    public function qqAction()
    {
        $qc = new QC();
        $acs = $qc->qq_callback();    // access_token
        $openid =  $qc->get_openid();     // openid
        //获取用户信息
        $qc = new QC($acs, $openid);
		$qqInfo = $qc->get_user_info();
    }
}