# Создание админской вкладки. 
- Вешаем обработчик на событие OnAdminTabControlBegin

```php
AddEventHandler("main", "OnAdminTabControlBegin", "OnAdminTabControlBeginHandler");
```
- Пример обработчика
```php
function MyOnAdminTabControlBegin(&$form) {
    if($GLOBALS["APPLICATION"]->GetCurPage() == "/bitrix/admin/user_edit.php" && (!empty($_REQUEST["ID"]) )
    {
        $form->tabs[] = array("DIV" => "additional_edit", "TAB" => "Дополнительно", "ICON"=>"main_user_edit", "TITLE"=>"Дополнительные параметры", "CONTENT"=>
            'тут любой контент который нужен на вкладке'
        );
    }
}

```
