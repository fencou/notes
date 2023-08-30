## 前台

`/modown-child/module/user-erphpdown.php`

### 用户中心

1. 添加一段css
```css
<style>
    <?php
        if ($userTypeId==9||$userTypeId==6){
            
        }else{
            echo "#charge-form, .charge-header, .charge-gift, .charge-tips, .usermenu-charge, .usermenu-history, .usermenu-withdraw {display:none;}";
        }
    ?>
</style>
```

2. 累计充值
```php
//sql查询添加 ice_note=0
$totalchong = $wpdb->get_var("SELECT SUM(ice_money) FROM $wpdb->icemoney WHERE ice_success=1 and ice_note=0 and ice_user_id=".$current_user->ID);
```

3. 余额明细
```php
//用户领取的不放+号
<tbody>
	              <?php foreach($lists as $value){?>
	            	  <tr>
	            	  	<td><?php echo (strpos($value->ice_note, '用户领取') !== false) ? $value->ice_money : (($value->ice_money > 0) ? '+' : '') . $value->ice_money; ?>
</td>
	                  	<td><?php echo $value->ice_note;?></td>
	                  	<td><?php echo $value->ice_time;?></td>
	                  </tr>
			      <?php }?>
	              </tbody>
```
```php
if(intval($value->ice_note)==0){echo "<td class=pc><font color=green>".__('在线充值','mobantu')."</font></td>\n";}elseif(intval($value->ice_note)==1){echo "<td class=pc>".__('后台充值','mobantu')."</td>\n";}elseif(intval($value->ice_note)==4){echo "<td class=pc><font color=orange>".__('Mycred兑换','mobantu')."</font></td>\n";}elseif(intval($value->ice_note)==6){echo "<td class=pc><font color=orange>".__('充值卡','mobantu')."</font></td>\n";}
//添加两行
elseif(intval($value->ice_note)==98){echo "<td class=pc><font color=orange>".__('红包冻结','mobantu')."</font></td>\n";}
elseif(intval($value->ice_note)==99){echo "<td class=pc><font color=orange>".__('红包解冻','mobantu')."</font></td>\n";}
else{echo "<td class=pc>".__('其他','mobantu')."</td>";}
```


## 后台

### vip

`/erphpdown/admin/erphp-vip-users.php`

```php
<table class="widefat fixed striped posts">
			<thead>
				<tr>
					<th>用户ID</th>
					<th>VIP类型</th>
					<!--添加一栏 -->
					<th>余额</th> 
					<th>到期时间</th>	
					<th>操作</th>
					
				</tr>
			</thead>
			<tbody>
				<?php
				if($list) {
					$erphp_life_name    = get_option('erphp_life_name')?'('.get_option('erphp_life_name').')':'';
					$erphp_year_name    = get_option('erphp_year_name')?'('.get_option('erphp_year_name').')':'';
					$erphp_quarter_name = get_option('erphp_quarter_name')?'('.get_option('erphp_quarter_name').')':'';
					$erphp_month_name  = get_option('erphp_month_name')?'('.get_option('erphp_month_name').')':'';
					$erphp_day_name  = get_option('erphp_day_name')?'('.get_option('erphp_day_name').')':'';
					foreach($list as $value){
					    //添加3个变量
					    $okm = erphpGetUserOkMoneyById($value->ice_user_id);
					    $user_name=get_the_author_meta( 'user_login', $value->ice_user_id );
						$cu = get_user_by('id',$value->ice_user_id);
						if($value->userType == 6) $typeName = '体验'.$erphp_day_name;
						else {$typeName=$value->userType==7 ?'包月'.$erphp_month_name :($value->userType==8 ?'包季'.$erphp_quarter_name : ($value->userType==10 ?'终身'.$erphp_life_name : '包年'.$erphp_year_name));}
						echo "<tr class=\"vip-$value->ice_id\">\n";
						echo "<td>";
						if($value->ice_user_id){
						    //输出会员名（昵称）
							echo '<a href="'.admin_url('admin.php?page=erphpdown%2Fadmin%2Ferphp-chong-list.php&type&username='.$user_name.'&order&date_start&date_over&payment=online') . '">'.$user_name.'</a>（昵称：'.$cu->nickname.'）';
						}else{
							echo '游客IP:'.$value->ice_ip;
						}
						echo "</td>";
						echo "<td>$typeName</td>";
						//输出金额
						echo '<td><a href="'.admin_url('admin.php?page=erphpdown%2Fadmin%2Ferphp-moneylog-list.php&username='.$user_name)  . '">'.$okm.'</a></td>';
						echo "<td><input type=text name=p_price_$value->ice_id id=p_price_$value->ice_id value=$value->endTime style=\"width:120px;\" /><input type=button id=editpricebtn_$value->ice_id onclick=editPrice($value->ice_id) value=修改 class=button></td>";
						echo '<td><a href="javascript:;" class="delvip" data-id="'.$value->ice_id.'" onclick="delvip('.$value->ice_id.')">删除VIP权限</a></td>';
						
						echo "</tr>";
					}
				}else{
					echo '<tr><td colspan="4" align="center"><strong>没有记录</strong></td></tr>';
				}
				?>
			</tbody>
		</table>
```

### 充值记录

`/erphpdown/admin/erphp-chong-list.php`

```php
if(isset($_GET['payment']) && $_GET['payment']){
	if($_GET['payment'] == 'card'){
		$where .= ' and ice_note=6';
	}elseif($_GET['payment'] == 'dashbord'){
		$where .= ' and ice_note=1';
	}elseif($_GET['payment'] == 'online'){
		$where .= ' and ice_note=0';
	}elseif($_GET['payment'] == 'mycred'){
		$where .= ' and ice_note=4';
	}elseif($_GET['payment'] == 'red-onlock'){
		$where .= ' and ice_note=98';
	}elseif($_GET['payment'] == 'red-unlock'){
		$where .= ' and ice_note=99';
	}
}
//40行 添加冻结和解冻的筛选
```

```php
<option value="red-onlock"<?php if(isset($_GET['payment']) && $_GET['payment'] == 'red-onlock') echo ' selected';?>>红包冻结</option>
				<option value="red-unlock"<?php if(isset($_GET['payment']) && $_GET['payment'] == 'red-unlock') echo ' selected';?>>红包解冻</option>
//85行添加筛选按钮
```

```php
// 173行附近表格中加入充值方式
elseif(intval($value->ice_note)==98)
					{
						echo "<td><font color=orange>红包冻结</font></td>\n";
					}
					elseif(intval($value->ice_note)==99)
					{
						echo "<td><font color=orange>红包解冻</font></td>\n";
					}

```