---
title: test
date: 2016-06-11 19:04:39
tags:
---

```php
class Test Extends CI_Controller {

	public function __construct() {
		parent::__construct();
	}

	// 同步跳转
	public function returnurl(){
	    $alipay_config = C('alipay');
	    //计算得出通知验证结果
	    $alipayNotify = new Alipay\AlipayNotify($alipay_config);
	    $verify_result = $alipayNotify->verifyReturn();
	    if($verify_result) {//验证成功
	        //商户订单号
	        $out_trade_no = $_GET['out_trade_no'];
	        //支付宝交易号
	        $trade_no = $_GET['trade_no'];
	        //交易状态
	        $trade_status = $_GET['trade_status'];
	            if($_GET['trade_status'] == 'TRADE_FINISHED' || $_GET['trade_status'] == 'TRADE_SUCCESS') {
	                // 下面是处理订单信息都代码，根据$out_trade_no商户订单号获取订单信息然后更改订单状态
	                $order = M('order');
	                $row = $order->where(array('number'=>$out_trade_no))->find();
	                if($row){
	                    if($order->where(array('id'=>$row['id']))->save(array('status'=>1))){
	                        $this->assign('row',$row);
	                        $this->display('buy_success');
	                    }else{
	                        $this->error('错误');    
	                    }
	                }else{
	                    $this->error('错误');
	                }
	            }else {
	              echo "trade_status=".$_GET['trade_status'];
	            }   
	            echo "验证成功<br />";
	    }else {
	        //验证失败
	        //如要调试，请看alipay_notify.php页面的verifyReturn函数
	        echo "验证失败";
	    }
	}
}
```