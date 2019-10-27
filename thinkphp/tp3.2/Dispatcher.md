## Dispatcher

> ·�ɷַ���

    App:init�н���Dispatcher::dispatch()
    
    ��Ҫ���û����ʵ�·���н�������·�ɽ���ģʽ
    
    �������û����ʵ�ģ��(Model)�Լ�����(Action)
    
    �������ģ���µ�һЩ��Ҫ�����ļ�
    
    �ٽ���App:exec()�з������н��
    

> dispatch��������ͼ

![image](./images/tp3.2-includeConfig.png)







## �������

```php

// �����ļ�ThinkPHP/Conf/convention.php   


//����ģʽPATHINFO��ȡ�������� ?s=/module/action/id/1 ����Ĳ���ȡ����URL_PATHINFO_DEPR

url�����Լ�·�ɹ���
        
```



## Dispatcher

> �ڲ������ֽ�

```php
static private function getController($var) {
    $controller = (!empty($_GET[$var])? $_GET[$var]:C('DEFAULT_CONTROLLER'));
    unset($_GET[$var]);
    if($maps = C('URL_CONTROLLER_MAP')) {
        if(isset($maps[strtolower($controller)])) {
            // ��¼��ǰ����       define('CONTROLLER_ALIAS',strtolower($controller));
            // ��ȡʵ�ʵĿ�������
            return   ucfirst($maps[CONTROLLER_ALIAS]);
        }elseif(array_search(strtolower($controller),$maps)){
            // ��ֹ����ԭʼ������
            return   '';
        }
    }

    if(C('URL_CASE_INSENSITIVE')) {
        // URL��ַ�����ִ�Сд
        // ����ʶ��ʽ user_type ʶ�� UserTypeController ������
        $controller = parse_name($controller,1);
    }
    return strip_tags(ucfirst($controller));
}



/**
 * ���ʵ�ʵĲ�������
 * @access private
 * @return string
 */
static private function getAction($var) {
    $action   = !empty($_POST[$var]) ?
        $_POST[$var] :
        (!empty($_GET[$var])?$_GET[$var]:C('DEFAULT_ACTION'));
    unset($_POST[$var],$_GET[$var]);
    if($maps = C('URL_ACTION_MAP')) {
        if(isset($maps[strtolower(CONTROLLER_NAME)])) {
            $maps =  $maps[strtolower(CONTROLLER_NAME)];
            if(isset($maps[strtolower($action)])) {
                // ��¼��ǰ����
                define('ACTION_ALIAS',strtolower($action));
                // ��ȡʵ�ʵĲ�����
                if(is_array($maps[ACTION_ALIAS])){
                    parse_str($maps[ACTION_ALIAS][1],$vars);
                    $_GET   =   array_merge($_GET,$vars);
                    return $maps[ACTION_ALIAS][0];
                }else{
                    return $maps[ACTION_ALIAS];
                }
                
            }elseif(array_search(strtolower($action),$maps)){
                // ��ֹ����ԭʼ����
                return   '';
            }
        }
    }        
    return strip_tags(strtolower($action));
}



/**
 * ���ʵ�ʵ�ģ������
 * @access private
 * @return string
 */
static private function getModule($var) {
    $module   = (!empty($_GET[$var])?$_GET[$var]:C('DEFAULT_MODULE'));
    unset($_GET[$var]);
    if($maps = C('URL_MODULE_MAP')) {
        if(isset($maps[strtolower($module)])) {
            // ��¼��ǰ����
            define('MODULE_ALIAS',strtolower($module));
            // ��ȡʵ�ʵ�ģ����
            return   ucfirst($maps[MODULE_ALIAS]);
        }elseif(array_search(strtolower($module),$maps)){
            // ��ֹ����ԭʼģ��
            return   '';
        }
    }
    return strip_tags(ucfirst(strtolower($module)));
} 
    
```