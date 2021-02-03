<template>
  <div id="app">
    <link rel="stylesheet" href="//at.alicdn.com/t/font_2289420_chlpl3n3w16.css">
    <router-view></router-view>
    <footer
        style=" background-color: #df5746; color: rgb(255, 255, 255); padding: 30px 50px;font-family:Consolas, 'Andale Mono', 'Lucida Console', 'Lucida Sans Typewriter', Monaco, 'Courier New', 'monospace';height: auto">
      <div style="display: flex; flex-direction: column; align-items: center;">
        <div style="margin: 12px 0;margin-bottom: 17px;">
          <table>
            <tr style="height: 35px;">
              <td>
                <!--                <img src="../vab_img/logo.jpg.png" alt="logo" style="width: auto; height:35px;">-->
              </td>
              <td style="height: 35px;line-height: 35px;color: #ffffff;font-weight: bold;font-size: 23px;letter-spacing: 3px">
                &nbsp;北航懂网
              </td>
            </tr>
          </table>
        </div>
        <div style="margin: 8px 0;">Copyright &copy;2020 by 6杯奶茶
        </div>
        <div style="margin: 8px 0;">若本站收录内容无意侵犯了贵公司版权，请给网页底部微信或邮箱地址来信，我们会及时处理和回复，谢谢合作</div>
        <div style="margin: 8px 0;">Email：920855608@qq.com</div>
        <div style="width: 300px;height: 1px; background-color: #FFFFFF;margin-top: 20px;"></div>
        <div style="margin: 13px 0;height: auto;">
          <a href="#" class="wechat">
            <div class="wechat-img iconfont icon-icon-weixin" style="font-size: 27px;color: #ffffff;text-align: center">

              <!--              <img src="//piccache.cnki.net/kdn/index/kns7/nimages/logo_wx.png" alt="微信" style="position: center">-->
            </div>
            <div style="border-radius: 10px; border-radius: 10px;color: #B2D0EE;">
              <img class="qrcode" src="../src/image/qrcode.jpg" alt="微信二维码">
            </div>
          </a>
        </div>

      </div>
    </footer>
  </div>
</template>
<style scoped rel="stylesheet">
.wechat:hover .qrcode {
  width: 120px;
  height: auto;
  border-radius: 10px;
  box-shadow: 0 0 15px;
  background-color: #f8ddd1;
  margin-top: 10px;
}

.qrcode {
  width: 0;
}

/*.wechat:hover .wechat-img {*/
/*  padding-left: 40px;*/
/*}*/
</style>
