SMTP

```
function MBT_mail_smtp( $phpmailer ) {
 $phpmailer->IsSMTP();
 $phpmailer->SMTPAuth = true;//启用SMTPAuth服务
 $phpmailer->Port = 465;//MTP邮件发送端口，这个和下面的对应，如果这里填写25，则下面为空白
 $phpmailer->SMTPSecure ="ssl";//是否验证 ssl，这个和上面的对应，如果不填写，则上面的端口须为25
 $phpmailer->Host = "smtp.qq.com";//邮箱的SMTP服务器地址，如果是QQ的则为：smtp.exmail.qq.com
 $phpmailer->Username = "quntools@qq.com";//你的邮箱地址
 $phpmailer->Password ="mhdfdbzyoxnygfca";//你的邮箱登录密码
 $phpmailer->FromName = '必销客'; // 发件人昵称
}
add_action('phpmailer_init', 'MBT_mail_smtp');

function MBT_wp_mail_from( $original_email_address ) {
 return 'quntools@qq.com'; //这个很重要，得将发件地址改成和上面smtp邮箱一致才行
}
add_filter( 'wp_mail_from', 'MBT_wp_mail_from' );
```