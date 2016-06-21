---
title: PHP多维数组转换为XML文件
s: convert-php-array-to-xml
date: 2016-06-21 14:01:59
tags: [xml]
categories: PHP
---

> 项目使用了百度的`站内搜索`需要提交`xml`文件。于是进行数据处理的时候是把符合搜索条件的数据整理成数组，然后导出为`xml`文件格式提交给百度。
>
> 使用的是`PHP`的`SimpleXMLElement`集成类库。

刚开始自己写的时候发现一般的数据格式(一维数组)很容易弄，后来百度那边要求的数据结构内嵌结构有4层。所以后来直接拿了一个别人写的类库来使用。
类库地址:[http://www.lalit.org/lab/convert-php-array-to-xml-with-attributes](http://www.lalit.org/lab/convert-php-array-to-xml-with-attributes)
<!-- more -->
由于时间比较紧，所以在使用这个库的时候遇到了一些问题。顶级`root`节点不知该如何添加.导致数据生成的格式为
```xml
<?xml version="1.0" encoding="UTF-8"?>
    <sitemap>
        <loc>http://bbb.com</loc>
        <lastmod>2016-06-21</lastmod>
    </sitemap>
    <sitemap>
        <loc>http://aaa.com</loc>
        <lastmod>2016-06-21</lastmod>
    </sitemap>
```
而正常的应该为
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex> root节点
    <sitemap>
        <loc>http://bbb.com</loc>
        <lastmod>2016-06-21</lastmod>
    </sitemap>
    <sitemap>
        <loc>http://aaa.com</loc>
        <lastmod>2016-06-21</lastmod>
    </sitemap>
</sitemapindex>
```
而且这个`Array2XML`使用的是PHP的`DomDocument`对象，所以没有去深究。但是这个库我测试了多维数组也有点问题，当嵌套的内部使用了索引数组时就会报错。后来找了一个别人修改版本**[原文链接](http://www.cnblogs.com/hbklove8/archive/2013/03/02/2940406.html)**

他在其中加入了一个`arrayLevel`的函数，用于返回数组纬度，然后在`createXML`方法中进行判断
```php
public static function &createXML($node_name, $arr=array()) {
    $xml = self::getXMLRoot();
    if( self::arrayLevel($arr)>1 ){ //这里加入了多维数组的判断
        foreach($arr as $key=>$val){
         $xml->appendChild(self::convert($node_name, $val));
        }
    }
    else {
        $xml->appendChild(self::convert($node_name, $arr));
    }

    self::$xml = null; // clear the xml node in the class for 2nd time use.
    return $xml;
}
```

```php
/**
* 返回数组的维度
* @param [type] $arr [description]
* @return [type] [description]
*/
private static function &arrayLevel($arr){
    $al = array(0);
    function aL($arr,&$al,$level=0){
        if(is_array($arr)){
            $level++;
            $al[] = $level;
            foreach($arr as $v){
                aL($v,$al,$level);
            }
        }
    }
    $al = aL($arr,$al);
    // return max($al); 这里报错，原因是不能直接返回引用而是使用变量
    return $al;
}
```

---
最后上面的我都没有使用，而是使用了自己写的，因为上面的类库虽然强大，但是有点复杂。我只是要一个导出的`xml`文件而已，其他功能不需要。

```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Zhannei_baidu extends Cron_Controller
{
    protected $dir_contest = 'sitemap/contest/';
    
    function __construct()
    {
        parent::__construct();
        $this->load->model('contest_m');
        $this->load->model('contest_content_m');

        $this->load->config('contest');

        switch (ENVIRONMENT) {
            case 'development':
                $this->site_url = 'http://my.xxx.dev/';
                break;
            case 'testing':
                $this->site_url = 'http://dev.xxx.com/';
                break;
            default:
                $this->site_url = 'http://www.xxx.com/';
                break;
        }
    }

    public function contest()
    {
        // dump(FCPATH);exit;
        // header('Content-type: text/xml');
        $start = 0;
        $limit = 5000;
        $where = [
            'status' => 1,
            // 'contest_type' => 2,
        ];
        $totalNum = $this->contest_m->get_count($where);
        $list = $this->contest_m->get_list($where, $start, $limit, ['contest_id', 'desc']);
        // 处理校级竞赛的URL地址
        $this->contest_m->make_contest_url($list);
        $i = 1;
        while (! empty($list)) {
            $xmlData = array_map(function($item)
            {
                $contest_content_info = $this->contest_content_m->get($item['contest_id']);
                if ($contest_content_info['content']) {
                    $contest_content = html_entity_decode($contest_content_info['content'], ENT_QUOTES);
                }
                else {
                    $contest_content = '省略';
                }


                if (! $item['web_pic_big']) {
                    $web_pic_big_arr = contest_rand_pic($item['contest_type'], $item['contest_id']);
                    $web_pic_big = $web_pic_big_arr[$item['contest_id']] . '?imageView2/1/w/140';
                } else {
                    $web_pic_big = $item['web_pic_big'] . '?imageView2/1/w/140';
                }

                // 分类
                $class = config_item('contest_category');
                $tag = ($item['contest_type']) ? $class[$item['contest_type']] : '自由';

                return [
                    'loc'        => $this->site_url . trim($item['contest_url'], '/'),
                    'lastmod'    => date('Y-m-d'),
                    'changefreq' => 'daily',
                    'priority'   => '0.5',
                    'data'       => [
                        'display' => [
                            'title'      => strip_tags($item['contest_name']),
                            'content'    => strip_tags($contest_content),
                            'tag'        => $tag,
                            'pubTime'    => date('c', strtotime($item['create_time'])),
                            'breadCrumb' => $this->site_url . 'vs',
                            'thumbnail'  => $web_pic_big,
                        ]
                    ],
                ];
            }, $list);

            $start += $limit;
            $list = $this->contest_m->get_list($where, $start, $limit, ['contest_id', 'desc']);

            $xml = new SimpleXMLElement('<?xml version="1.0" encoding="UTF-8"?><urlset />');
            $this->array_to_xml($xmlData, $xml, 'url');
            $xml->asXML($this->dir_contest . 'contest_'. $i . '.xml');
            
            // 生成sitemapindex
            $index_xml = new SimpleXMLElement('<?xml version="1.0" encoding="UTF-8"?><sitemapindex />');
            $indexData[] = [
                'loc' => $this->site_url . $this->dir_contest . 'contest_' . $i . '.xml',
                'lastmod' => date('Y-m-d'),
            ];

            $i++;
        }
        $this->array_to_xml($indexData, $index_xml, 'sitemap');
        $index_xml->asXML(FCPATH . 'sitemap.xml');
        
    }

    /**
     * 转换数组toxml
     * @date   2016-06-20
     */
    public function array_to_xml($data, &$xml, $child = 'item')
    {
        foreach($data as $key => $value) {
            if(is_array($value)) {
                if ( ! is_numeric($key)) {
                    $subnode = $xml->addChild($key);
                    $this->array_to_xml($value, $subnode);
                }
                else {
                    $subnode = $xml->addChild($child);
                    $this->array_to_xml($value, $subnode);
                }
            }
            else {
                $xml->addChild("$key",htmlspecialchars("$value"));
            }
        }
    }
    
}

/* End of file Data2xml.php */
/* Location: ./application/controllers/Data2xml.php */
```

PS: XML转换为PHP数组的库，也是上面的那位大神写的[XML转换为PHP数组](http://www.lalit.org/lab/convert-xml-to-array-in-php-xml2array)



